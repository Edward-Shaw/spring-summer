# Spring Security 搭建简单的权限管理系统
基于 `Spring Boot` 搭建权限管理系统
## 1. 权限管理需求说明
1. 定义资源
2. 定义操作权限
3. 根据资源和对该资源可进行的操作权限创建角色
4. 创建用户

### 1. 为操作定义权限
- 新建
- 修改
- 查看
- 删除

系统涉及权限概括来看就“CURD”四个。权限直接具有上下级关系，例如如果用户被授予了“删除”权限，那么该用户同时也
自动获取到了“查看”权限。

### 2. 定义资源
- 
- 超级管理员权限，可以使用所有操作
- 委托单
    - 新建
    - 审核
    - 修改
    - 删除(作废)
    - 详情查看
- 样品
    - 新建
    - 出入库
    - 修改
    - 删除(作废)
    - 详情查看
- 检测机构
    - 新建
    - 修改
    - 删除(停用)
    - 详情查看
- 检测项目
    - 新建
    - 预约检测
    - 修改
    - 删除(停用)
    - 详情查看
- 客户
    - 新建
    - 修改
    - 删除(禁用)
    - 详情
- 权限
    - 详情
- 机构用户
    - 新建
    - 权限赋予
    - 删除(禁用)
    - 修改
    - 查看
- 系统用户
    - 新建
    - 权限赋予
    - 修改
    - 删除(禁用)
    
### 2. 为系统用户设置用户名、密码
### 3. 创建权限组，分配权限
### 4. 将系统用户添加到权限组中
## 2. 数据库设计
## 3. 实施
### 1. 增加Spring Security依赖
在`pom.xml`中加入以下内容：
``` java
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-web</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-config</artifactId>
</dependency>
```
### 2. 配置Spring Security
新建`WebSecurityConfig.java`文件
