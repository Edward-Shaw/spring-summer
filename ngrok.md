# Ubuntu下编译安装ngrok
在微信开发过程中，调试过程极不方便，后来偶然了解到 [ngrok](https://github.com/inconshreveable/ngrok) 这个东西，用起来实在顺手！国内有很多先驱已经提供了免费的ngrok服务，人都是有惰性，
果断用别人的免费服务，后来用的人多了，问题也多了，这东西渐渐不稳定。最近产品要上线测试，结果正赶上ngrok服务很不稳定，没办法，只好自己来搭建一个了。好久没写博客了，借此机会更新一下！
## 1. 导出源代码
```
mkdir ngrok
apt-get update
apt-get install git
git clone https://github.com/inconshreveable/ngrok.git
```
PS. 直接在服务器上下载的话实在太慢，可以先在本地下载好，然后用ftp放到服务器上去直接用。
Tips. ftp客户端要选择 `站点管理器`，同时使用 `SFTP - SSH FIle Transfer protocol` 协议，否则还要去设置ftp server，一般sftp都是默认的。
## 2. 安装Golang
```
apt-get install golang
```
## 3. 更改ngrok域名
```
export GOPATH=/usr/local/ngrok/
export NGROK_DOMAIN="ngrok.yourdomain.com"
```
PS. subdomain随意就好，推荐 `ngrok` 或者 `tunnel`
## 4. 为域名生成证书
```
openssl genrsa -out rootCA.key 2048
openssl req -x509 -new -nodes -key rootCA.key -subj "/CN=$NGROK_DOMAIN" -days 5000 -out rootCA.pem
openssl genrsa -out server.key 2048
openssl req -new -key server.key -subj "/CN=$NGROK_DOMAIN" -out server.csr
openssl x509 -req -in server.csr -CA rootCA.pem -CAkey rootCA.key -CAcreateserial -out server.crt -days 5000
```
## 5. 拷贝证书到指定位置
```
cp rootCA.pem assets/client/tls/ngrokroot.crt
cp server.crt assets/server/tls/snakeoil.crt
cp server.key assets/server/tls/snakeoil.key
```
## 6. 编译
```
make release-server release-client
GOOS=windows GOARCH=amd64 make release-client
```
PS. 第一句会编译好server，即 `ngrokd`，还有一个linux的客户端 `ngrok`。不过我们一般开发的时候都需要用到Windows平台的，再运行第二句就可以编译出一个Windows版本的 `ngrok.exe`。
同样的，使用ftp工具下载到本地即可。
## 7. 运行Server
```
./ngrokd -tlsKey="../assets/server/tls/snakeoil.key" -tlsCrt="../assets/server/tls/snakeoil.crt" -domain="ngrok.yourdomain.com"
```
PS. 在运行命令的最后加上 `&` 可以让程序在后台默默运行，然后把控制台让出来干其他活。
服务运行起来后直接在浏览器中输入你的ngrok域名测试服务是否正常启动，如果看到 `Tunnel xxx.ngrok.yourdomain.com not found` 的提示，那就成功了！
## 8. 运行Client
按照我上面的编译命令，应该生成了一个 `windows_amd64` 文件夹，该文件夹里面只有一个 `ngrok.exe` 文件，你还需要在该文件夹中新建一个 `ngrok.cfg`配置文件。
配置文件里面加上如下内容：
```
server_addr: "ngrok.yourdomain.com:4443"
trust_host_root_certs: false
```
PS. `server_addr`的值必须和上面第3步输入的域名一致，否则Server端会出现 `remote error: bad certificate `的错误输出，然后Client端将一直处于`connecting...`的状态。
接下来命令行输入命令运行Client：
```
ngrok.exe -subdomain yourdomain -config=ngrok.cfg port
```
出现绿色的`Tunnel Status: online`就OK了！



    
