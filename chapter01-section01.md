# Spring

### 什么是框架

> 框架是前人长期经验实践出的一套遵循了一定的设计模式和规范设计出的松耦合，可拓展，可维护的产品，它可以帮助我们简单，快速，高效的解决某一领域的问题。

### Spring简介

##### 发展史

* 2000年，Spring框架最开始的部分是由Rod Johnson于2000年为伦敦的金融界提供独立咨询业务时写出来的。

* 2002年，Rod Johnson在2002年编著的《Expert one on one J2EE design and development》一书中，对Java EE 系统框架臃肿、低效、脱离现实的种种现状提出了质疑，并积极寻求探索革新之道

* 2003年2月，一批自愿拓展Spring框架的程序开发员组成了团队，在Sourceforge上构建了一个项目。Spring开始兴起

* 2003年6月，Spring的框架首次在Apache 2.0的使用许可中发布

* 2004年3月，在Spring框架上工作了一年之后，这个团队发布了第一个版本（1.0）

  同年他又推出了一部堪称经典的力作《Expert one-on-one J2EE Development without EJB》，该书在Java世界掀起了轩然大波，不断改变着Java开发者程序设计和开发的思考方式。

  Rod Johnson成为一个改变Java世界的大师级人物

* 2005年，Spring框架的开发人员成立了自己的公司，来提供对Spring的商业支持，其中最显著的就是与BEA的合作。

  第一个Spring会议在迈阿密举行，3天的课程吸引了300名开发人员。2006年6月在安特卫普召开的会议有400多名开发人员。

##### 简介

* Spring是一个开源框架，轻量级的Java 开发框架
* Spring是一个开源框架，轻量级的Java 开发框架
* Spring通过一种称作控制反转（IoC）的技术促进了低耦合
* Spring提供了面向切面编程的丰富支持
* MVC——Spring的作用是整合，但不仅仅限于整合

##### 优势

* 方便解耦，简化开发
* 独立于各种应用服务器（支持在WebLogic，Tomcat，Resin，JBoss，WebSphere和其他的应用服务器上的用户）
* AOP编程的支持（Spring的AOP支持允许将一些通用任务如安全、事务、日志等进行集中式管理，从而提供了更好的复用）
* 声明式事务的支持（在Spring中，我们可以从单调烦闷的事务管理代码中解脱出来，通过声明式方式灵活地进行事务的管理，提高开发效率和质量）
* 方便集成各种优秀框架（Spring不排斥各种优秀的开源框架，相反，Spring可以降低各种框架的使用难度，Spring提供了对各种优秀框架（如Struts,Hibernate、Quartz）等的直接支持）
* 降低Java EE API的使用难度（Spring对很多难用的Java EE API（如JDBC，JavaMail，远程调用等）提供了一个薄薄的封装层，通过Spring的简易封装，这些Java EE API的使用难度大为降低）

### 体系结构

> Spring 有可能成为所有企业应用程序的一站式服务点,Spring框架提供很多模块，可以根据应用程序的要求来使用

![Spring核心架构图](img/chapter01-section01-001.png)

##### 核心容器

​	核心容器由**spring-core，spring-beans，spring-context，spring-context-support和spring-expression**（SpEL，Spring表达式语言，Spring Expression Language）等模块组成，它们的细节如下：

* **spring-core**模块提供了框架的基本组成部分，包括 IoC 和依赖注入功能
* **spring-beans** 模块提供 BeanFactory，工厂模式的微妙实现，它移除了编码式单例的需要，并且可以把配置和依赖从实际编码逻辑中解耦
* **context**模块建立在由**core**和 **beans** 模块的基础上建立起来的，它以一种类似于JNDI注册的方式访问对象。Context模块继承自Bean模块，并且添加了国际化（比如，使用资源束）、事件传播、资源加载和透明地创建上下文（比如，通过Servelet容器）等功能。Context模块也支持Java EE的功能，比如EJB、JMX和远程调用等。**ApplicationContext**接口是Context模块的焦点。**spring-context-support**提供了对第三方库集成到Spring上下文的支持，比如缓存（EhCache, Guava, JCache）、邮件（JavaMail）、调度（CommonJ, Quartz）、模板引擎（FreeMarker, JasperReports, Velocity）等
* **spring-expression**模块提供了强大的表达式语言，用于在运行时查询和操作对象图。它是JSP2.1规范中定义的统一表达式语言的扩展，支持set和get属性值、属性赋值、方法调用、访问数组集合及索引的内容、逻辑算术运算、命名变量、通过名字从Spring IoC容器检索对象，还支持列表的投影、选择以及聚合等

##### 数据访问/集成

​	数据访问/集成层包括 JDBC，ORM，OXM，JMS 和事务处理模块，它们的细节如下：（注：JDBC=Java Data Base Connectivity，ORM=Object Relational Mapping，OXM=Object XML Mapping，JMS=Java Message Service）

* **JDBC** 模块提供了JDBC抽象层，它消除了冗长的JDBC编码和对数据库供应商特定错误代码的解析
* **ORM** 模块提供了对流行的对象关系映射API的集成，包括JPA、JDO和Hibernate等。通过此模块可以让这些ORM框架和spring的其它功能整合，比如前面提及的事务管理
* **OXM** 模块提供了对OXM实现的支持，比如JAXB、Castor、XML Beans、JiBX、XStream等
* **JMS** 模块包含生产（produce）和消费（consume）消息的功能。从Spring 4.1开始，集成了spring-messaging模块
* **事务**模块为实现特殊接口类及所有的 POJO 支持编程式和声明式事务管理。（注：编程式事务需要自己写beginTransaction()、commit()、rollback()等事务管理方法，声明式事务是通过注解或配置由spring自动处理，编程式事务粒度更细）

##### WEB

​	Web 层由 Web，Web-MVC，Web-Socket 和 Web-Portlet 组成，它们的细节如下：

* **Web** 模块提供面向web的基本功能和面向web的应用上下文，比如多部分（multipart）文件上传功能、使用Servlet监听器初始化IoC容器等。它还包括HTTP客户端以及Spring远程调用中与web相关的部分
* **Web-MVC** 模块为web应用提供了模型视图控制（MVC）和REST Web服务的实现。Spring的MVC框架可以使领域模型代码和web表单完全地分离，且可以与Spring框架的其它所有功能进行集成
* **Web-Socket** 模块为 WebSocket-based 提供了支持，而且在 web 应用程序中提供了客户端和服务器端之间通信的两种方式
* **Web-Portlet** 模块提供了用于Portlet环境的MVC实现，并反映了spring-webmvc模块的功能

##### 其他

​	其他一些重要的模块，像 AOP，Aspects，Instrumentation，Web 和测试模块，它们的细节如下：

* **AOP** 模块提供了面向方面的编程实现，允许你定义方法拦截器和切入点对代码进行干净地解耦，从而使实现功能的代码彻底的解耦出来。使用源码级的元数据，可以用类似于.Net属性的方式合并行为信息到代码中
* **Aspects** 模块提供了与 **AspectJ** 的集成，这是一个功能强大且成熟的面向切面编程（AOP）框架
* **Instrumentation** 模块在一定的应用服务器中提供了类 instrumentation 的支持和类加载器的实现
* **Messaging** 模块为 STOMP 提供了支持作为在应用程序中 WebSocket 子协议的使用。它也支持一个注解编程模型，它是为了选路和处理来自 WebSocket 客户端的 STOMP 信息
* **测试**模块支持对具有 JUnit 或 TestNG 框架的 Spring 组件的测试

