# spring

## 简介

简化我们的工作，获取对象的方式发生极大的改变

```java
Girl girl = new Girl();
//写死的方式
new PrettyGirl();
new YoungGirl();
```

```java
Pay pay;

pay = WXPay;
pay = AliPay;
```

大部分对象应该从容器中获取，而不是进行java硬编码



容器：就由它来写，以后我们要什么，就从这个容器里面拿，在容器中去声明告诉它，给我准备什么。

## 基础技术

- java
- 反射
- xml
- xml解析
- 代理
- 大量设计模式



## 基础环境搭建

1. 添加spring的依赖
2. 编写一个spring配置文件
3. 通过spring的应用程序上下文对象获取对象



依赖

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-context</artifactId>
  <version>5.0.8.RELEASE</version>
</dependency>
```

spring配置文件：applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--
        把对象的创建交给spring容器，在这个配置文件里面去声明我要什么对象
        class：写java类的全限定名，它是通过全类名然后使用反射的技术进行创建的
        id：给这个对象在整个应用程序上下文当中取个名字以方便区分
    -->
    <!--这种方式就称之为注入-->
    <bean class="com.fanstudy.pojo.Girl" id="girl">
    </bean>
</beans>
```



```xml
配置一：
<bean class="com.fanstudy.pojo.Girl" id="girl">
</bean>

配置二：
<bean class="com.fanstudy.pojo.PrettyGirl" id="girl">
</bean>
```



```java
@Test
public void m1(){
    //1，获取上下文对象，spring配置文件里面声明的对象需要通过上下文对象获取
    ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");

    //2. 通过这个对象获取我们的girl
    Girl girl = (Girl) ctx.getBean("girl");
    System.out.println(girl);
}

输出一：
//om.fanstudy.pojo.Girl@7ce6a65d

输出二：
//com.fanstudy.pojo.PrettyGirl@7ce6a65d
```



## 普通编写 VS spring方式

​	普通的获取对象的方式，所有对象之间的依赖，类之间的依赖关系都是在java代码里面维护的，很难维护。如果我们有替换方案，替换比较困难。

​	对象产生全是在配置文件里面完成的，我们想分析关系直接在配置文件里面就看出来了。



核心内容学习：

- IOC
- AOP



IOC概念：

​	控制反转：inverse of control	什么控制，谁反转了谁

控制：创建对象，彼此关系的权利

控制权是在开发人员在程序代码当中掌控的。new

夺取控制权

反转给spring容器

- 声明要什么
- spring容器来进行具体的控制

改变了编程的方式

### 容器

![](C:\Users\14601\Desktop\images\container-magic.png)

- pojos：自己定义的这些类
- metadata：在spring的配置文件里面写的这些就是元数据
- 实例化容器：classpath将配置文件传入，实例化完毕

### 值的注入

```xml
<!--注入，配置元数据-->
<!--
    值的注入：
-->
<bean class="com.fanstudy.pojo.Girl" id="girl">
    <!--name指定要注入值的名称 value给其赋值-->
    <property name="lastName" value="王菲"/>
</bean>
```

- setter注入（最常用的）
  - 必须其字段有对应的setter方法才可以完成。name	setName
  - 通过property子节点注入。
- 构造注入

  - 方式一：

    ```xml
    <bean class="com.fanstudy.pojo.Orders" id="orders">
        <constructor-arg name="id" value="1"/>
        <constructor-arg name="name" value="aaa"/>
        <constructor-arg name="total" value="123.4"/>
    </bean>
    ```

  - 方式二：

    ```xml
    <bean class="com.fanstudy.pojo.Orders" id="orders2">
        <constructor-arg index="0" value="1"/>
        <constructor-arg index="1" value="aaa"/>
    </bean>
    最好不要用
    ```

  - 方式三：按照构造函数里面入参的类型

    ```xml
    <bean class="com.fanstudy.pojo.Orders" id="orders2">
        <constructor-arg type="java.lang.Double" value="1"/>
        <constructor-arg type="java.lang.String" value="aaa"/>
    </bean>
    ```


如果没有对应的setter方法

```java
Invalid property 'lastName' of bean class [com.fanstudy.pojo.Girl]: Bean property 'lastName' is not writable or has an invalid setter method.
```

注意

默认是通过无参构造器完成对象的创建的

如果没有无参构造器

```java
No default constructor found; nested exception is java.lang.NoSuchMethodException: 
```

## bean元素探讨

属性探讨

- abstract：改bean将无法被无法被实例化

- parent：指定它的父bean是谁，将会继承父bean的所有内容，通过id指引

  ```xml
  <bean class="com.fanstudy.pojo.Girl" id="girl" abstract="true">
      <property name="lastName" value="王菲"/>
  </bean>
  
  <bean class="com.fanstudy.pojo.Girl" id="youngGirl" parent="girl">
      <property name="flower" value="牡丹"/>
  </bean>
  ```


- destroy-method：指定这个bean最后销毁的时候一定执行的方法，适合与清理型工作，触发条件是必须bean确实是被销毁才发生。

  - 容器close会触发
  - refresh会触发

- init-method：指定bean的初始化方法，准备性工作

- name：别名，可以通过它获取。可以多个，彼此分隔方式空格，逗号等

- alias：单独定义别名

  ```xml
  <alias name="dog" aliens="adog"/>
  ```

- scope：指定范围

  - singleton：单例，**spring上下文**当中只有一个实例
  - prototype：原型：要一个就新给一个

- lazy-init：true就是spring一上来不会直接初始化我们的bean，当我们要是用的时候才会初始化。

  - 直接初始化（默认）
    - 应用程序启动慢一些，内存消耗大一些
    - 当我们使用bean的时候快一些
  - 延迟初始化

- depend-on：依赖的bean，如果某一个bean严重依赖于另一个bean的准备的话，就可以配置。

```xml
<bean class="com.fanstudy.pojo.Girl" id="girl" depend-on="dog">
    <!--非字面值描述的属性的注入，通过ref(引用)指向另外一个bean的id-->
    <property name="dog" ref="dog"/>
</bean>

<bean class="com.fanstudy.pojo.Dog" id="dog">
    <property name="name" value="哮天犬"/>
</bean>
```

spring多个配置文件的bean是可以相互引用的（前提是配置文件扫描）