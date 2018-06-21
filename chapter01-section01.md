### Spring概述



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

优势

* 方便解耦，简化开发
* 独立于各种应用服务器（支持在WebLogic，Tomcat，Resin，JBoss，WebSphere和其他的应用服务器上的用户）
* AOP编程的支持（Spring的AOP支持允许将一些通用任务如安全、事务、日志等进行集中式管理，从而提供了更好的复用）
* 声明式事务的支持（在Spring中，我们可以从单调烦闷的事务管理代码中解脱出来，通过声明式方式灵活地进行事务的管理，提高开发效率和质量）
* 方便集成各种优秀框架（Spring不排斥各种优秀的开源框架，相反，Spring可以降低各种框架的使用难度，Spring提供了对各种优秀框架（如Struts,Hibernate、Quartz）等的直接支持）
* 降低Java EE API的使用难度（Spring对很多难用的Java EE API（如JDBC，JavaMail，远程调用等）提供了一个薄薄的封装层，通过Spring的简易封装，这些Java EE API的使用难度大为降低）

