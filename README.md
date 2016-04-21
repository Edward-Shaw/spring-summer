# spring-summer
My spring-framework reading notes.PS,finish it before summer ends.

*** To be continue: *** 

## 1. setup
1. Download source code from [GitHub](https://github.com/spring-projects/spring-framework.git)
2. Install [Gradle](http://gradle.org/gradle-download/) and setup Gradle path.
3. Convent source code to Eclipse project: `gradle cleanidea eclipse`

## 2. 核心组件
![Spring Framework](/images/spring.jpg)
### 1. Core Container
> Core、Beans、Context和Expression Language

Core和Beans模块是框架基础部分，提供IoC(控制反转)和依赖注入特性。其基础是BeanFactory，提供对Factory模式的经典实现来消除对程序性单例模式的需要，
从而真正允许从程序逻辑中分离出依赖关系和配置。
### 2. Data Access/Integration
> JDBC、ORM、OXM、JMS和Transaction

- JDBC包含Spring对JDBC数据访问进行封装的所有类，其消除了繁琐的JDBC编码和解析数据库厂商特有的错误代码。
- ORM为流行的对象-关系映射API，如JPA、JDO、Hibernate和iBatis等，提供了一个交互层。
- OXM提供了对Object/XML映射实现的抽象层，包括JAXB、Castor、XMLBeans、JiBX和XStream.
- JMS(Java Messaging Service)包含制造和消费消息的特性。
- Transaction模块支持编程和声明性的事务管理，事务类必须实现特定的接口并且对所有的POJO都适用。

### 3. Web
> Web、Web-Servlet、Web-Struts和Web-Porlet

- Web提供了基础的面向Web的集成特性：多文件上传、使用servlet listeners初始化IoC容器以及面向Web的应用上下文。
- Web-Servlet包含Spring的model-view-controller实现。
- Web-Struts提供对Struts的支持，使得类在Spring应用中能够与一个典型的Struts Web层集成。
- WebPorlet提供了用于Porlet环境和Web-Servlet模块的MVC实现。

### 4. AOP
> Aspects、Instrumentation

 - 提供了一个符合AOP联盟标准的面向切面编程的实现，它让你可以定义例如方法拦截器和切点，从而将逻辑代码分开，降低它们的耦合性。通过配置管理特性，Spring AOP
 模块直接将面向切面的编程功能集成到了Spring框架中，所以可以很容易地使Spring框架管理的任何对象支持AOP。Spring AOP模块为基于Spring的应用程序中的对象提供了事务管理服务。
 通过使用Spring AOP，不用依赖EJB组件，就可以将声明性事务管理集成到应用程序中。
 
### 5. Test
- Test模块支持使用JUnit和TestNG对Spring组件进行测试。