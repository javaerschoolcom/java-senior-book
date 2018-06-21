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

### Spring中的Bean

>  bean 是一个被实例化，组装，并通过 Spring IoC 容器所管理的对象,是应用程序的支柱

#### Bean配置

> Spring中提供了三种Bean的配置

* 基于XML的配置（常用）
* 基于注解的配置（常用）
* 基于JAVA的配置（不常用）

##### 基于XML的配置元数据

###### 每一个bean用```<bean>```节点表示，该节点有如下常用配置属性

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

###### 每一个```<bean>```节点的子节点```<property>```配置,用于初始化bean的依赖(又称作setter注入，需提供setter方法)

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

