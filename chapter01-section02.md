# Spring核心之IoC

### Ioc概念

> ​	引入：在每天的日常生活中，我们离不开水，电，气。在城市化之前，我们每家每户需要自己去搞定这些东西：自己挖水井取水，自己点煤油灯照明，自己上山砍柴做饭。而城市化之后，人们从这些琐事中解放了出来，城市中出现了水厂，发电厂，燃气公司。水，电，气我们自己打开开关用就可以而不用关心这些都是怎么来的，怎么实现的。控制反转指的就是由我们自己提供水电气发展为由政府提供水电气的整个过程。

##### IoC定义

​	控制反转（Inversion of Control，简称IOC）

​	指的是应用本身不负责依赖对象的创建及维护：当我们创建一个对象或者调用对象的方法时，不再主动去创建这个类的对象，依赖对象的创建及维护是由外部容器（Spring）负责的

##### Ioc理解

 	控制反转是一种将组件依赖关系的创建和管理置于程序外部的技术。
	由容器控制程序之间的关系，而不是由代码直接控制
	由于控制权由代码转向了容器，所以称为反转

##### 依赖注入

​	依赖注入（Dependency Injection，简称DI）

​	什么是依赖：对象与对象之间的关系可以简单的理解为对象之间的依赖关系
        依赖关系：在 A 类需要类 B 的一个实例来进行某些操作，比如在类 A 的方法中需要调用类 B 的方法来完成功能，叫做 A 类依赖于 B 类。
	spring主动创建被调用类的对象，然后把这个对象注入到我们自己的类中，使得我们可以使用它

##### 总结

​	依赖注入(Dependency Injection)和控制反转(Inversion of Control)是同一个概念，后者是一种思想，前者是对这种思想的实现。
	具体含义是:当某个角色(可能是一个Java实例，调用者)需要另一个角色(另一个Java实例，被调用者)的协助时，在 传统的程序设计过程中，通常由调用者来创建被调用者的实例。但在Spring里，创建被调用者的工作不再由调用者来完成，因此称为控制反转;创建被调用者 实例的工作通常由Spring容器来完成，然后注入调用者，因此也称为依赖注入。

##### 优点

​        削减计算机程序的耦合问题，也是轻量级的Spring框架的核心

### 入门程序案例

> 使用Spring提供的Ioc实现方式创建管理对象

* 引入Spring的核心jar包

``` java
spring-beans-4.3.18.RELEASE.jar
spring-context-4.3.18.RELEASE.jar
spring-core-4.3.18.RELEASE.jar
spring-expression-4.3.18.RELEASE.jar
commons-logging日志包
log4j日志包
```

* 创建一个简单JAVA类

``` java
public class HelloWorld {
   private String message;
   public void setMessage(String message){
      this.message  = message;
   }
   public void getMessage(){
      System.out.println("Your Message : " + message);
   }
}
```

* 创建Spring核心配置文件spring-config.xml

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

   <bean id="helloWorld" class="lero.HelloWorld">
       <property name="message" value="Hello World!"/>
   </bean>
</beans>
```

* 创建测试类

``` java
public class Test {
   public static void main(String[] args) {
      //Spring读取配置信息
      ApplicationContext context = new ClassPathXmlApplicationContext("spring-config.xml");
      //从Spring的Ioc容器中找到指的的bean
      HelloWorld obj = (HelloWorld) context.getBean("helloWorld");
      //调用bean对应的方法
      obj.getMessage();
   }
}
```

## Spring中的Bean

>  bean 是一个被实例化，组装，并通过 Spring IoC 容器所管理的对象,是应用程序的支柱

#### Bean配置

> Spring中提供了三种Bean的配置

* 基于XML的配置（常用）
* 基于注解的配置（常用）
* 基于JAVA的配置（不常用）

#### 基于XML的配置元数据

##### 每一个bean用```<bean>```节点表示，该节点有如下常用配置属性

* id属性：给bean指定一个名称，通过该名称可以取到bean

``` xml
<bean id="helloWorld"></bean>
```

* name属性:给bean指定一个或者多个名称，通过该名称可以取到bean，作用和id属性一样

``` xml
<bean name="hello,world"></bean>
```

* class属性：指定bean的全限定名称

``` xml
<bean id="helloWorld" class="lero.HelloWorld"></bean>
```

* scope属性：指定bean的作用域，默认的是单例Singleton的，若每一次调用都是一个新的对象请使用prototype

``` xml
<bean id="helloWorld" class="lero.HelloWorld" scope="prototype"></bean>
```

* init-method属性：指定bean初始化的方法,bean初始化的时候会调用该方法

``` xml
<bean id="helloWorld" class="lero.HelloWorld" init-method="before"></bean>
<!--需要在lero.HelloWorld类中，定义一个方法before -->
```

* destory-method属性：指定bean摧毁的方法,bean释放的时候会调用该方法

``` xml
<bean id="helloWorld" class="lero.HelloWorld" init-method="after"></bean>
<!--需要在lero.HelloWorld类中，定义一个方法after -->
```

* lazy-init属性:指定bean是否在spring加载的时候就初始化bean,默认非懒加载

``` xml
<bean id="helloWorld" class="lero.HelloWorld" lazy-init="true"></bean>
<!--在使用该bean的时候，Spring容器才负责创建该对象 -->
```

* abstract属性：指定bean是否是抽象的，抽象的bean一般没有class属性，而是作为模板存在
* parent属性:指定要继承自哪一个bean的配置（在类上可以不存在继承关系）

``` xml
<bean id="template" abstract="true">
   <property name="message1" value="Hello"/>
   <property name="message2" value="Hello"/>
</bean>
<bean id="test1" class="lero.Test1" parent="template"></bean>
<bean id="test2" class="lero.Test2" parent="template"></bean>
<!--test1,test2均继承自template的注入属性，相当于test1,test2也注入了message1，message2属性-->
```

* autowire属性：指定bean是否自动装配，装配的属性值有byName，byType,constructor常用

``` xml
<bean id="helloWorld" class="lero.HelloWorld" autowire="byName"></bean>
```

* autowire-candidate属性：指定是否是一个候选待装配的bean,默认为true,设为false则取消候选

``` xml
<bean id="helloWorld" class="lero.HelloWorld" autowire-candidate="false"></bean>
<bean id="helloWorld2" class="lero.HelloWorld"></bean>
<!--如果需要HelloWorld类型的bean的时候，helloWorld2会被装配，helloWorld则不会被装配 -->
```

* primary属性：设置自动装配byType类型，指定的首选装配bean

``` xml
<bean id="helloWorld" class="lero.HelloWorld" primary="true"></bean>
<bean id="helloWorld2" class="lero.HelloWorld" primary="false"></bean>
<!--
	如果autowired是byType类型，spring根据类型查找bean，当一个bean的primary为true的，那该bean在autowired是byType时 就是首选。spring根据primary的信息就会把这个bean拿去注入。而不会在找到多个相同类型的bean时，spring因为不知道到底该注入哪个baen而抛异常
 -->
```

* depends-on属性：表示一个bean A的实例化依靠另一个bean B的实例化，首先应该等待beanB初始化完成,如果是单例的，摧毁的时候应该先摧毁beanA

```xml
<bean id="beanA" class="lero.BeanA" depends-on="beanB" scope="singleton" ></bean>
<bean id="beanB" class="lero.BeanB" ></bean>
```

* factory-bean属性：指定工厂的bean
* factory-method属性：指定使用工厂创建指定bean的方法的名称

> 案例：非静态工厂属性factory-bean&factory-method配置

``` JAVA
public class FruiFactory {
    public   Fruit  newApple(){
        return  new Apple();
    }
    public   Fruit  newOrange(){
        return  new Orange();
    }
}
```

``` java
public interface Fruit {
    void eat();
}
```

``` java
public class Apple implements  Fruit {
    @Override
    public void eat() {
        System.out.println("我是苹果，可以直接吃");
    }
}
```

``` java
public class Orange implements  Fruit {
    @Override
    public void eat() {
        System.out.println("我是橙汁，我要剥皮吃");
    }
}
```

``` XML
 <bean id="factory" class="lero.FruiFactory"></bean>
 <bean id="apple"  factory-bean="factory" factory-method="newApple"></bean>
 <bean id="orange" factory-bean="factory" factory-method="newOrange"></bean>
```

``` java
public class Test {
    public static void main(String[] args) {
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("spring-
        Fruit apple =(Fruit)context.getBean("apple");
        apple.eat();
        Fruit orange =(Fruit)context.getBean("orange");
        orange.eat();
    }
}
```

> 案例：静态工厂属性factory-bean&factory-method配置

``` java
public class FruiFactory {
    public  static Fruit  newApple(){
        return  new Apple();
    }
    public  static Fruit  newOrange(){
        return  new Orange();
    }
}
```

``` xml
 <bean id="apple" class="FruiFactory" factory-method="newApple"></bean>
 <bean id="orange" class="FruiFactory" factory-method="newOrange"></bean>
```

##### setter注入：每一个```<bean>```节点的子节点```<property>```配置,用于初始化bean的依赖(需提供setter方法)

* 初始化基本数据类型和String类型配置
* 初始化自定义类类型配置
* 初始化Array,List,Map,Properties配置

> 注入主类Persion.java

``` java
public class Persion {
    //注入Properties类型
    private Properties  children;
    //注入List类型
    private List friends;
    //注入Set类型
    private Set foes;
    //注入Map类型
    private Map<String,Animal> pets;
    //注入自定义类类型
    private  Tool tool;
    //注入String类型
    private String name;
    //注入基本数据类型
    private int age;
    public Properties getChildren() {
        return children;
    }
    public void setChildren(Properties children) {
        this.children = children;
    }
    public List getFriends() {
        return friends;
    }
    public Set getFoes() {
        return foes;
    }
    public void setFoes(Set foes) {
        this.foes = foes;
    }
    public Map<String, Animal> getPets() {
        return pets;
    }
    public void setPets(Map<String, Animal> pets) {
        this.pets = pets;
    }
    public void setFriends(List friends) {
        this.friends = friends;
    }
    public Tool getTool() {
        return tool;
    }
    public void setTool(Tool tool) {
        this.tool = tool;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    @Override
    public String toString() {
        return "Persion{" +
                "children=" + children +
                ", friends=" + friends +
                ", foes=" + foes +
                ", pets=" + pets +
                ", tool=" + tool +
                ", name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

> 注入的依赖类Animal.java

``` java
public class Animal {
    private  String name;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    @Override
    public String toString() {
        return "Animal{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

> 注入的依赖类Tool.java

``` java
public class Tool {
    private String name;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    @Override
    public String toString() {
        return "Tool{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

> 核心配置spring-config.xml

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

       <bean id="persion" name="xxx,qqq,xdd" class="Persion">
            <property name="age" value="18"></property>
            <property name="name" value="lero"></property>
            <property name="tool" ref="tool"></property>
            <property name="friends">
                  <list>
                        <ref bean="dog"></ref>
                        <ref bean="cat"></ref>
                        <value>ls</value>
                        <value>zs</value>
                  </list>
            </property>
            <property name="pets">
                  <map>
                        <entry key="pet1" value-ref="cat"></entry>
                        <entry key="pet2" value-ref="dog"></entry>
                  </map>
            </property>
            <property name="foes">
                  <set>
                        <value>xx</value>
                        <value>oo</value>
                  </set>
            </property>
            <property name="children">
                  <props>
                        <prop key="child1">child1</prop>
                        <prop key="child2">child2</prop>
                  </props>
            </property>
      </bean>

      <bean id="tool" class="Tool">
            <property name="name" value="axe"></property>
      </bean>

      <bean id="dog" class="Animal">
            <property name="name" value="dog"></property>
      </bean>
      <bean id="cat" class="Animal">
            <property name="name" value="cat"></property>
      </bean>
</beans>
```

> 测试类Test.java

``` java
public class Test {
    public static void main(String[] args) {
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("spring-config.xml");
        Persion persion = (Persion)context.getBean("persion");
        System.out.println(persion.toString());
    }
}
```

* 注入内部bean

``` xml
 <bean id="outerBean" class="...">
      <property name="target">
         <bean id="innerBean" class="..."/>
      </property>
 </bean>
```

* 注入NUll值

```xml
<bean class="TestBean">
	<property name="email"><null/></property>
    <!--等同于testBean.setEmali(null) -->
</bean>
```

##### 构造器注入：每一个```<bean>```节点的子节点```<constructor-arg>```配置构造器,用于初始化bean的依赖

``` java
public class A {
    private B b;
    private String s;
    private  A(B param1){
        this.b=param1;
    }
    private  A(B param1,String param2 ){
        this.b=param1;
        this.s=param2;
    }
}
```

``` java
public class B {
}
```

> 使用name属性匹配

``` xml
<bean id="a" class="A">
      <constructor-arg name="param1" ref="b"/>
      <!--匹配一个参数的构造器-->
</bean>
<bean id="b" class="B"></bean>
```

> 使用index属性匹配,下面两个都能注入

``` xml
<bean id="a" class="A">
      <constructor-arg index="1" value="lero"/>
      <constructor-arg index="0" ref="b"/>
      <!--匹配两个参数的构造器，参数的顺序为索引的顺序-->
</bean>
<bean id="b" class="B"></bean>
```

``` xml
<bean id="a" class="A">
      <constructor-arg index="0" ref="b"/>
      <constructor-arg index="1" value="lero"/>
      <!--匹配两个参数的构造器，参数的顺序为索引的顺序-->
</bean>
<bean id="b" class="B"></bean>
```

> 使用type属性匹配，下面两个都能注入，但是若构造器多个参数的类型一样，配置的顺序将影响注入的顺序，建议按照顺序配置

``` xml
<bean id="a" class="A">
       <constructor-arg type="java.lang.String" value="lero"/>
       <constructor-arg type="B" ref="b"/>
       <!--匹配两个参数的构造器，参数为索引指定的顺序-->
 </bean>
 <bean id="b" class="B"></bean>
```

``` xml
<bean id="a" class="A">
       <constructor-arg type="B" ref="b"/>
       <constructor-arg type="java.lang.String" value="lero"/>
       <!--匹配两个参数的构造器，参数为索引指定的顺序-->
 </bean>
 <bean id="b" class="B"></bean>
```

> 使用默认配置，建议按构造器参数的顺序，配置constructor-arg的顺序

``` xml
<bean id="a" class="A">
       <constructor-arg  ref="b"/>
       <constructor-arg  value="lero"/>
       <!--匹配两个参数的构造器，参数为索引指定的顺序-->
 </bean>
 <bean id="b" class="B"></bean>
```

##### 静态工厂注入（java代码参考factory-bean属性配置的案例）

> 工厂方法有参数：假设更改上面的案例方法为newApple(String clazz);

``` java
 <bean id="apple" class="FruiFactory" factory-method="newApple">
 	<constructor-arg name="clazz" value="lero.Apple"/>
 </bean>
 <bean id="orange" class="FruiFactory" factory-method="newOrange">
 	<constructor-arg name="clazz" value="lero.Orange"/>
 </bean>
```

##### 非静态工厂注入（java代码参考factory-bean属性配置的案例）

> 工厂方法有参数：假设更改上面的案例方法为newApple(String clazz);

``` xml
 <bean id="factory" class="lero.FruiFactory"></bean>
 <bean id="apple"  factory-bean="factory" factory-method="newApple">
	 <constructor-arg name="clazz" value="lero.Apple"/>
 </bean>
 <bean id="orange" factory-bean="factory" factory-method="newOrange">
	<constructor-arg name="clazz" value="lero.Orange"/>
 </bean>
```

##### 基于注解的配置：

* 开启支持注解自动注入（需要引入context的schema约束）

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
       ">
      <!--开启支持注解自动注入 -->
       <context:annotation-config />
</beans>
```

* @Required

> 适用于bean属性setter方法，并表示受影响的bean属性必须在XML配置文件进行配置。否则，容器会抛出一个BeanInitializationException异常

* @Autowired

> 用在setter和构造器上只按照byType注入（如果注释属性名和bean一直，则按byName注入），用在成员变量field上，先按照byName注入，找不到则按照byType注入

* @Qualifier

> 如果@Autowired注入有多个类型的实现，则可指定按照bean的name过滤装配指定类型bean，配合Autowired 一起使用

* @Resource

> 自动装配注入顺序如下：
>
> 　 (1). 如果同时指定了name和type，则从Spring上下文中找到唯一匹配的bean进行装配，找不到则抛出异常;
> 　 (2). 如果指定了name，则从上下文中查找名称（id）匹配的bean进行装配，找不到则抛出异常;
> 　 (3). 如果指定了type，则从上下文中找到类型匹配的唯一bean进行装配，找不到或者找到多个，都会抛出异常;
> 　 (4). 如果既没有指定name，又没有指定type，则自动按照byName方式进行装配；如果没有匹配，则回退为一个原始类型进行匹配，如果匹配则自动装配。
> @Resource分别可以用在field、setter上，无法应用在constructor构造器上

