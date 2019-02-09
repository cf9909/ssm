# springmvc

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.1.3.RELEASE</version>
</dependency>
```

![](C:\Users\14601\Desktop\images\1.JPG)



![](C:\Users\14601\Desktop\images\2.JPG)

## springmvc的依赖

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.0.8.RELEASE</version>
</dependency>

<!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>
    <scope>provided</scope>
</dependency>
```

## 入门体验

1. 创建web项目
2. 编写web.xml，在其中注册一个特殊的servlet，前端控制器
3. 编写springmvc配置文件
4. 编写控制器
5. 编写结果页面

## web.xml

注册前端控制器

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="3.0" xmlns="http://java.sun.com/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd">

  <servlet>
    <!--如果不去修改spring配置文件的默认位置，那么springmvc会去WEB-INF下找springmvc-servlet.xml文件-->
    <servlet-name>springmvc</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  </servlet>

  <!--servlet映射配置-->
  <servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <!--统一写/-->
    <url-pattern>/</url-pattern>
  </servlet-mapping>

</web-app>
```



## springmvc配置文件

springmvc-servlet.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--配置一个视图解析器
    常用内部资源视图解析器
    -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!--前缀-->
        <property name="prefix" value="/jsp/"/>
        <!--后缀-->
        <property name="suffix" value=".jsp"/>
    </bean>
</beans>
```

springmvc配置文件名字

默认是用DispatcherServlet的名字当作命名空间

[namespace].xml	(WEB-INF下寻找)



springmvc配置文件springmvc-servlet.xml

如果改为mvc-servlet.xml

```
Could not open ServletContext resource [/WEB-INF/springmvc-servlet.xml]
```

解决可将web.xml

```
<servlet>
  <!--如果不去修改spring配置文件的默认位置，那么springmvc会去WEB-INF下找springmvc-servlet.xml文件-->
  <servlet-name>springmvc</servlet-name>
  <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  <init-param>
    <param-name>namespace</param-name>
    <param-value>mvc-servlet</param-value>
  </init-param>
</servlet>
```



如果将springmvc配置文件(springmvc.xml)放到resources下，web.xml下配置<servlet>

```xml
<init-param>
  <!--上下文配置的位置的制定-->
  <param-name>contextConfigLocation</param-name>
  <param-value>classpath:springmvc.xml</param-value>
</init-param>
```



### 视图解析器

- 视图前缀
  - /jsp/
- 视图后缀
  - .jsp
- viewName（如girl）
  - /jsp/girl.jsp

逻辑视图

- prefix
- logicViewName
- suffix

物理视图由逻辑视图转换而来

WEB-INF/jsp/girl.jsp



## 注解开发模式

基本注解

- @Controller
- @RequestMapping

springmvc.xml配置注解扫描的包

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                          http://www.springframework.org/schema/beans/spring-beans.xsd
                          http://www.springframework.org/schema/context
                          http://www.springframework.org/schema/context/spring-context.xsd">

    <!--配置一个注解扫描的包-->
    <context:component-scan base-package="com.fanstudy.controller"/>
```



```java
package com.fanstudy.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class ByeController {

    @RequestMapping("/bye")
    public String bye(Model model) {
        model.addAttribute("model", "modeller");
        return "bye";
    }
}
```

@RequestMapping

方法上、类上，推荐使用二者结合的方式

## 转发与重定向

```java
package com.fanstudy.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping("/hello")
public class HelloController {

    //转发
    @RequestMapping("/forward")
    public String forward(Model model){
        System.out.println(111111);
        model.addAttribute("skill", "睡觉");
        return  "forward";
    }

    //重定向
    @RequestMapping("/redirect")
    public String redirect(Model model){
        model.addAttribute("skill", "煮饭");
        System.out.println(222222);
        //不带/的写法是一个相对路径，根据当前上下文决定，hello
        //如果是重定向，跟视图解析规则无关
        return "redirect:/jsp/redirect.jsp";
    }

    //转发到另一个控制器
    @RequestMapping("forward2")
    public String forwardAnotherController(){
        return "forward:/baby/hello";
    }
}
```

转发：http://localhost:8080/hellomvc/hello/forward

重定向：http://localhost:8080/hellomvc/jsp/redirect.jsp?skill=%3F%3F



- 转发到页面	默认
- 重定向到页面  redirect:path
- 转发到另一个控制器  forward:path

## 关于springmvc访问web元素

- request
- session



## 注解详解

### @RequestMapping

- value是一个数组形式，可以匹配多个路径

  ```
  //请求的路径path可以多个值
  @RequestMapping(value = {"/m1", "/m2"})
  ```

- path 是value的别名，二者选其一

- method指定可以访问的请求的类型，比如get，post，它也可以写成一个数组的形式

  ```
  @RequestMapping(path = {"/m1", "/m2"}, method = RequestMethod.GET)//如果没限定，啥请求都可以
  ```

- params可以去指定参数，还可以限定参数的特征，等于某个值，不等于某个值

  ```
  @RequestMapping(path = {"/m2"}, params = {"girl", "boy"})
  @RequestMapping(path = {"/m2"}, params = {"girl=wangfei", "boy!=guandong"})
  ```

  - http://localhost:8080/hellomvc/web/m2?girl=123&boy=456

- headers  能够影响浏览器的行为

  ```
  Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
  Accept-Encoding: gzip, deflate, br
  Accept-Language: zh-CN,zh;q=0.9
  Cache-Control: no-cache
  Connection: keep-alive
  Cookie: JSESSIONID=D6FD6740431F61C321FFBC8E31FBBB07; JSESSIONID=026F0648B7ACC28B5F6EB14A9FE0D08C
  Host: localhost:8080
  Pragma: no-cache
  Upgrade-Insecure-Requests: 1
  User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36
  ```

- consumes  消费者，媒体类型，可以限定必须为application/json，charset=UTF-8

- produces  产生的响应的类型

### 关于请求路径的问题

springmvc支持ant风格

- ? 	任意一个字符，/除外

  ```java
  @RequestMapping(path = {"/m3?"})
  http://localhost:8080/hellomvc/web/m3a
  ```

- `*`  0到n，任意个字符，/除外

  ```java
  @RequestMapping(path = {"/m3*"})
  localhost:8080/hellomvc/web/m3nihaoma
  ```

- ** 支持任意层路径。/m3/** 可以体现出来；/m3**  效果等同于m3后面任意个字符

  ```java
  @RequestMapping(path = {"/m3/**"})
  ```

### @GetMapping、@PostMapping......

- GetMapping 限定了只支持get请求
- PostMapping 限定了只支持post请求 

### 对于非get、post请求的支持

需要有额外的内容添加，需添加一个过滤器来额外处理

- 过滤器

  web.xml中注册一个支持所有http请求类型的过滤器

  ```xml
  <!--注册一个支持所有http请求类型的过滤器-->
  <filter>
    <filter-name>hiddenHttpMethodFilter</filter-name>
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
  </filter>
  <filter-mapping>
    <filter-name>hiddenHttpMethodFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>
  ```

- 表单提交里面还要添加一个隐藏的参数

  ```h&#39;t
  <form action="${path}/web/m4" method="post">
      <input type="hidden" name="_method" value="DELETE">
      <input type="submit" value="提交">
  </form>
  ```



### @PathVariable

restful风格

```java
通过名字绑定这个路径中的变量
@RequestMapping("/add/{id}/{name}")
public String addProduct(@PathVariable("id") Integer id, @PathVariable("name") String name)
```

### @RestController

@ResponseBody + @Controller

### @ResponseBody

返回数据时，一般情况下返回json格式

json数据，不是通过form表单传递

### @InitBinder

数据转换，将日期转换处理

### @RequestParam

### @SessionAttributes（存）

用在类上面，它会将模型数据自动填充到session里面

### @SessionAttribute（检查）

要求当前这次访问会话中必须要有某个对象

```java
@Controller
@RequestMapping("/dog")
@SessionAttributes("dog")
public class DogControll<form action="${path}/web/m4" method="post">
    <input type="hidden" name="_method" value="DELETE">
    <input type="submit" value="提交">
</form>er {

    @RequestMapping("/register")
    public String register(Dog dog){
        return "dog";
    }
    @RequestMapping("/login")
    //它是检查当前这个对话里面是否有dog
    public String login(@SessionAttribute Dog dog){
        return "redirect:/jsp/dog.jsp";

    }
}
```



### @ModelAttribute

使用方式一

```java
//就是在controller里面的，任意一个处理具体的方法之前都会执行
@ModelAttribute
public User init(){
    System.out.println("init...");
    User u = new User();
    u.setName("王菲");
    return u;
}

@RequestMapping("/login")
public String login(Model model){
    System.out.println(model.containsAttribute("u"));
    System.out.println(model.containsAttribute("user"));
    System.out.println(model.containsAttribute("hsdfh"));
    return "msg";
    
    
init...
false
true
false
```

如果某些对象从头到尾每次请求都要存在，不消失，就适合这么用

使用方式二

```java
@ModelAttribute
public void init(Model model){
    System.out.println("init...");
    User user = new User();
    user.setName("王菲");
    model.addAttribute("user", user);
}
@RequestMapping("/login")
public String login(Model model){
    System.out.println(model.containsAttribute("u"));
    System.out.println(model.containsAttribute("user"));
    System.out.println(model.containsAttribute("hsdfh"));
    return "msg";
}
```



```java
@ModelAttribute
public void init(Model model){
    System.out.println("init...");
    User user = new User();
    user.setName("王菲");
    model.addAttribute("user", user);
}
//这种方式会直接去我们模型里面去找
@RequestMapping("/login")
public String login(@ModelAttribute User user){
    System.out.println(user.getName() +  user.getPassword());
    return "msg";
}
```

如果没有传递这个模型过来，加了@ModelAttribute的为你提供；

如果传了就用你的。



## 关于静态资源的访问问题

由于servlet设置了URL匹配方式为/，所以它将静态资源也当作一个后台的请求

比如http://localhost:8080/hellomvc/static/css/index.css

它尝试去匹配一个static/css/index.css的Controller里面的RequestMapping的组合，因为没有，所以404，找不到。

解决方法很多，最简单的，是让springmvc单独处理，将这些教给容器默认的servlet处理，不让DispatcherServlet来处理了。

解决方式1

```
springmvc配置文件中添加
<!--默认的servlet处理者-->
<mvc:default-servlet-handler/>

<!--
只加上面一个的话，相当于全部让它处理了
为了让原有的Controller生效，要加一个注解驱动
-->
<mvc:annotation-driven/>
```

MIME类型

解决方式2

通过映射关系描述，一一编写规则

```
<mvc:resources mapping="/static/css/*" location="static/css/"/>
```

解决方式3

自行在web.xml定义映射规则



## 关于post请求中文乱码问题解决

我么添加一个过滤器即可，springmvc提供了非常好的字符编码过滤器，所以我们在web.xml注册即可

```
  <filter>
    <filter-name>characterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <!--指定字符编码-->
    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
    
    <init-param>
      <param-name>forceRequestEncoding</param-name>
      <param-value>true</param-value>
    </init-param>
  </filter>
  
  <filter-mapping>
    <filter-name>characterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>
```



## 关于form表单提交数据的方式

### 方式一  通过名字绑定

通过属性名称进行绑定，可以完成数据传递

页面当中表单元素name值要和后台后台形参名字保持一致

如果有多个，多个形参通过名字绑定即可，当传入值较多时比较麻烦

<form action="${path}/user/put" method="post">
    <input type="hidden" name="_method" value="put">
    <input type="name" name="name">
    <input type="password" name="password">
    <input type="submit" value="提交">
</form>

```java
@Controller
@RequestMapping("/user")
public class UserController {
    @PutMapping("/put")
    @ResponseBody
    public String put(String name，String password){
        System.out.println(name + password);
        return "ok";
    }
}
```



### 方式二   利用@RequestParam

jsp页面不变

后台

```java
    @PutMapping("/put")
    @ResponseBody
    public String put(@RequestParam("name") String name, @RequestParam("password") String password){
        System.out.println(name + password);
        return "ok";
    }
```

### 方式三  直接使用pojo形式传递

jsp页面不变

后台

```java
    @PutMapping("/put")
    @ResponseBody
    public String put(User user){
        System.out.println(user.getName() + user.getPassword());
        return "ok";
    }
```



## 关于form表单提交日期格式问题的处理

1.处理日期（没有时间）

```java
    @InitBinder("user")
    public void init(WebDataBinder dataBinder){
        //这里指定什么格式，前台只能传什么格式
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-mm-dd");
        sdf.setLenient(false);
        dataBinder.registerCustomEditor(Date.class, new CustomDateEditor(sdf, false));
    }


    @PostMapping("/put")
    @ResponseBody
    public String put(@ModelAttribute("user") User user){
        System.out.println(user.getName() + user.getPassword());
        System.out.println(user.getBirthday());
        return "ok";
    }
//通过InitBinder指定user名字和ModelAttribute里面user绑定
```

2.不指定名字，根据数据类型一样可以分析解析转换成功

```java
  	@InitBinder
    public void init(WebDataBinder dataBinder){
        //这里指定什么格式，前台只能传什么格式
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-mm-dd");
        sdf.setLenient(false);
        dataBinder.registerCustomEditor(Date.class, new CustomDateEditor(sdf, false));
    }


    @PostMapping("/put")
    @ResponseBody
    public String put(@ModelAttribute User user){
        System.out.println(user.getName() + user.getPassword());
        System.out.println(user.getBirthday());
        return "ok";
    }
```

3.时间加日期

```java
    @InitBinder
    public void init(WebDataBinder dataBinder){
        //这里指定什么格式，前台只能传什么格式
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-mm-dd hh:mm:ss");
        sdf.setLenient(false);
        dataBinder.registerCustomEditor(Date.class, new CustomDateEditor(sdf, false));
    }


    @PostMapping("/put")
    @ResponseBody
    public String put(@ModelAttribute User user){
        System.out.println(user.getName() + user.getPassword());
        System.out.println(user.getBirthday());
        return "ok";
    }
```

4.在属性上面添加额外的注解

```java
//    @DateTimeFormat(pattern = "yyyy-mm-dd")
    @DateTimeFormat(pattern = "yyyy-mm-dd hh:mm:ss")
    private Date birthday;
```



## JSON数据交互

### 额外依赖

```xml
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.9.3</version>
</dependency>

<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-core</artifactId>
    <version>2.9.3</version>
</dependency>

<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-annotations -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-annotations</artifactId>
    <version>2.9.3</version>
</dependency>

<!-- https://mvnrepository.com/artifact/net.sf.json-lib/json-lib -->
<dependency>
    <groupId>net.sf.json-lib</groupId>
    <artifactId>json-lib</artifactId>
    <version>2.4</version>
    <classifier>jdk15</classifier>
</dependency>
<!--添加处理json为javabean-->
<!-- https://mvnrepository.com/artifact/org.codehaus.jackson/jackson-core-asl -->
<dependency>
    <groupId>org.codehaus.jackson</groupId>
    <artifactId>jackson-core-asl</artifactId>
    <version>1.9.2</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.codehaus.jackson/jackson-mapper-asl -->
<dependency>
    <groupId>org.codehaus.jackson</groupId>
    <artifactId>jackson-mapper-asl</artifactId>
    <version>1.9.2</version>
</dependency>
<!--添加处理json为javabean ===end===-->
```

另外记得添加

```xml
<!--激活springmvc消息转换功能-->
<mvc:annotation-driven/>
```

### JSON数据返回前台以及如何解析

#### JSON后台返回

1.返回pojo

```java
@RequestMapping("/m1")
@ResponseBody//这个注解将知道现在返回的不是视图，它会将这个数据转换为JSON格式
public User m1(){
    User u = new User();
    u.setName("zhao");
    u.setPassword(123456);
    return u;
}
```

```json
{
  "name": "zhao",
  "password": 123456
}
```



2.返回Map

```java
@RequestMapping("/m2")
@ResponseBody
public Map<String, Object> m2(){
    Map<String, Object> map = new HashMap<>();
    map.put("name", "李四");
    map.put("age", 28);
    return map;
}
```

```json
{
  "name": "李四",
  "age": 28
}
```

3.返回数组

```java
@RequestMapping("/m3")
@ResponseBody
public User[] m3(){
    User u = new User();
    u.setName("开玩笑");
    u.setPassword(123);
    User u2 = new User();
    u2.setName("不开玩笑");
    u2.setPassword(321);
    return new User[]{u,u2};
}
```



```json
[
  {
    "name": "开玩笑",
    "password": 123
  },
  {
    "name": "不开玩笑",
    "password": 321
  }
]
```



4.返回List

```java
@RequestMapping("/m4")
@ResponseBody
public List<User> m4(){
    List<User> list = new ArrayList<>();
    User u = new User();
    u.setName("开玩笑");
    u.setPassword(123);
    User u2 = new User();
    u2.setName("不开玩笑");
    u2.setPassword(321);
    list.add(u);
    list.add(u2);
    return list;
}
```

```json
[
  {
    "name": "开玩笑",
    "password": 123
  },
  {
    "name": "不开玩笑",
    "password": 321
  }
]
```

#### JSON前台如何解析

1. 解析返回的pojo

   ```js
   $('#b1').click(function () {
       $.ajax({
           url:'${path}/json/m1',
           type:'post',
           success:function (data) {
               alert(data.name);
               alert(data.password);
           }
       })
   })
   ```

2. 解析返回的Map

   ```js
   $('#b2').click(function () {
       $.ajax({
           url:'${path}/json/m2',
           type:'post',
           success:function (data) {
               alert(data.name);
               alert(data.age);
           }
       })
   })
   ```

3. 解析返回的数组

   ```js
   $('#b3').click(function () {
       $.ajax({
           url:'${path}/json/m3',
           type:'post',
           success:function (data) {
               for(var i = 0; i < data.length; i++){
                   alert(data[i].name);
                   alert(data[i].password);
               }
           }
       })
   })
   ```

4. 解析返回的List

   ```js
   $('#b4').click(function () {
       $.ajax({
           url:'${path}/json/m4',
           type:'post',
           success:function (data) {
               for(var i = 0; i < data.length; i++){
                   alert(data[i].name);
                   alert(data[i].password);
               }
           }
       })
   })
   ```

5. 返回List<Map<String,User>>

   ```js
   $('#b5').click(function () {
       $.ajax({
           url:'${path}/json/m5',
           type:'post',
           success:function (data) {
               for(var i = 0; i < data.length; i++){
                   alert(data[i].u1.name);
                   alert(data[i].u1.password);
                   alert(data[i].u2.name);
                   alert(data[i].u2.password);
               }
           }
       })
   })
   ```

   ```json
   [
     {
       "u1": {
         "name": "开玩笑",
         "password": 123
       },
       "u2": {
         "name": "不开玩笑",
         "password": 321
       }
     },
     {
       "u1": {
         "name": "林青霞",
         "password": 5667
       },
       "u2": {
         "name": "林冲",
         "password": 8837
       }
     }
   ]
   ```


### JSON数据如何使用Ajax提交到后台及如何解析

```java
contentType:'application/json'
```

否则数据无法识别

1. 前台写法

   ```js
   $(function () {
       $('#b1').click(function () {
           var obj = {
               'name':"叶问",
               'password':123
           }
           $.ajax({
               url:'${path}/json2/add',
               type:'post',
               data:JSON.stringify(obj),
               contentType:'application/json',
               success:function (data) {
   
               }
           })
       })
   })
   ```

2. 后台写法

   1. 参数直接和POJO绑定的方法

   ```java
   //前台如何提交一个User对象过来
   @RequestMapping("/add")
   //User user入参只能处理表单提交的数据
   public void add(@RequestBody User user){
       System.out.println(user.getName() + user.getPassword());
   }
   ```

   2. json其实可以理解为键值对，所以我们用Map接收，然后对字符串或者其他数据类型进行进一步处理。

   ```java
   @RequestMapping("/add")
   public void add(@RequestBody Map<String,Object> map){
       String name = map.get("name").toString();
       String password = map.get("password").toString();
       System.out.println(name + password);
   }
   ```



   一定记得添加@RequestBody

   ![1547990164315](C:\Users\14601\AppData\Roaming\Typora\typora-user-images\1547990164315.png)



   ## 关于form数据提交与Ajax自定义JSON数据提交的区别

   form提交方式

   ![1547990290212](C:\Users\14601\AppData\Roaming\Typora\typora-user-images\1547990290212.png)

   Ajax自定义JSON数据提交方式

   ![1547990164315](C:\Users\14601\AppData\Roaming\Typora\typora-user-images\1547990164315.png)

   对于form表单提交数据Content-Type: application/x-www-form-urlencoded

   对于ajax发送JSON则是Content-Type: application/json

1. 发送一个pojo

   前台

   ```js
   $('#b1').click(function () {
       var obj = {
           'name':"叶问",
           'password':123
       }
       $.ajax({
           url:'${path}/json2/add',
           type:'post',
           data:JSON.stringify(obj),
           contentType:'application/json',
           success:function (data) {
   
           }
       })
   })
   ```

   后台

   ```java
   //前台如何提交一个User对象过来
   @RequestMapping("/add")
   //User user入参只能处理表单提交的数据
   @ResponseBody
   public String add(@RequestBody User user){
       System.out.println(user.getName() + user.getPassword());
       return "msg";
   }
   ```

2. 发送一组pojo

   前台

   ```js
   $('#b2').click(function () {
       var obj = {
           'name':"叶问",
           'password':123
       };
       var obj2 = {
           'name':"霍元甲",
           'password':456
       };
       var arr = new Array();
       arr.push(obj);
       arr.push(obj2);
       $.ajax({
           url:'${path}/json2/addList',
           type:'post',
           data:JSON.stringify(arr),
           contentType:'application/json',
           success:function (data) {
               alert(data.code);
           }
       })
   }
   ```

   后台

   ```java
   @RequestMapping("/addList")
   @ResponseBody
   public Map<String,Integer> add(@RequestBody List<User> list){
       System.out.println(list);
       Map<String,Integer> map = new HashMap<>();
       map.put("code",2000);
       return map;
   }
   ```

   ## XML数据交互

   对于很多第三方开发，有很多会采用XML作为数据交互，比如微信

1. 添加额外的依赖

   ```xml
   <!--XML数据处理-->
   <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.dataformat/jackson-dataformat-xml -->
   <dependency>
     <groupId>com.fasterxml.jackson.dataformat</groupId>
     <artifactId>jackson-dataformat-xml</artifactId>
     <version>2.9.3</version>
   </dependency>
   ```

2. 方法返回数据类型的定义

   ```java
   //描述生产类型，返回类型，返回什么数据
   @RequestMapping(value = "/m1", produces = {MediaType.APPLICATION_XML_VALUE})
   @ResponseBody
   public User m1(){
       //将数据转换为xml形式的User
       User u = new User();
       u.setName("chenfan");
       u.setPassword(123456);
       return u;
   }
   ```

   ```xml
   <User>
   <name>chenfan</name>
   <password>123456</password>
   </User>
   ```



   如果我要实现如下效果可以吗？

   ```xml
   <User name>
       <name>chenfan</name>
       <password>123456</password>
   </User>
   ```





## 文件上传

### Apache Commons `FileUpload`

单文件

1. 添加依赖

   ```xml
   <!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
   <dependency>
       <groupId>commons-fileupload</groupId>
       <artifactId>commons-fileupload</artifactId>
       <version>1.3.3</version>
   </dependency>
   ```

2. 在springmvc.xml中注册一个文件上传解析器

   ```xml
   <!--文件上传解析器
   id必须为multipartResolver，原因是源代码里面写死为这个名字
   -->
   <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
       <!--定义最大上传大小 总的 单位为byte-->
       <property name="maxUploadSize" value="1024*1024"></property>
       <!--指定上传的编码-->
       <property name="defaultEncoding" value="UTF-8"></property>
       <!--单个文件最大上传大小-->
       <property name="maxUploadSizePerFile" value="2000000"></property>
   </bean>
   ```

3. 准备一个上传的页面

   ```jsp
   <%--添加一个enctype="multipart/form-data"--%>
   <form action="${path}/file/upload" method="post" enctype="multipart/form-data">
       文件：<input type="file" name="file"><br>
       <input type="submit" value="提交">
   </form>
   ```

4. 后台处理程序

   ```java
   @Controller
   @RequestMapping("/file")
   public class FileController {
   
       private static String uploadPath = "/home/cf/Downloads" + File.separator;
   
   	//入参就可以代表上传的文件
       @RequestMapping("/upload")
       public String upload(@RequestParam("file")MultipartFile multipartFile, Model model) throws IOException {
           //1.传到哪里去 2.传什么东西 3.传的细节
           
           //1.判断 不空才传
           if(multipartFile != null && !multipartFile.isEmpty()){
               //2.获取原始文件名
               String originalFilename = multipartFile.getOriginalFilename();
               //3.获取原文件名前缀，不带后缀
               String fileNamePrefix = originalFilename.substring(0, originalFilename.lastIndexOf("."));
               //4.加工处理文件名，将源文件名+时间戳
               String newFileNamePrefix = fileNamePrefix + new Date().getTime();
               //5.得到新文件名
               String newFileName = newFileNamePrefix + originalFilename.substring(originalFilename.lastIndexOf("."));
               //6.构建文件对象
               File file = new File(uploadPath + newFileName);
               //7.上传
               multipartFile.transferTo(file);
               model.addAttribute("fileName", newFileName);
           }
           return "uploadSuc";
       }
   }
   ```

多文件

- 前端

  ```jsp
  <%--多文件上传--%>
  <form action="${path}/file/upload2" method="post" enctype="multipart/form-data">
      文件：<input type="file" name="file"><br>
      文件：<input type="file" name="file"><br>
      文件：<input type="file" name="file"><br>
      文件：<input type="file" name="file"><br>
      文件：<input type="file" name="file"><br>
      <input type="submit" value="提交">
  </form>
  ```

- 后台

  ```java
  @RequestMapping("/upload2")
  public String upload(@RequestParam("file")MultipartFile[] multipartFiles, Model model) {
      List<String> fileNames = new ArrayList<>();
      //1.传到哪里去 2.传什么东西 3.传的细节
      //1.判断 不空才传
      if(multipartFiles != null && multipartFiles.length > 0){
          for (MultipartFile multipartFile : multipartFiles){
              //2.获取原始文件名
              String originalFilename = multipartFile.getOriginalFilename();
              //3.获取原文件名前缀，不带后缀
              String fileNamePrefix = originalFilename.substring(0, originalFilename.lastIndexOf("."));
              //4.加工处理文件名，将源文件名+时间戳
              String newFileNamePrefix = fileNamePrefix + new Date().getTime();
              //5.得到新文件名
              String newFileName = newFileNamePrefix + originalFilename.substring(originalFilename.lastIndexOf("."));
              //6.构建文件对象
              File file = new File(uploadPath + newFileName);
              //7.上传
              try {
                  multipartFile.transferTo(file);
                  fileNames.add(newFileName);
              } catch (IOException e) {
                  e.printStackTrace();
              }
          }
      }
      model.addAttribute("fileNames", fileNames);
      return "uploadSuc";
  }
  ```

  ###  Servlet 3.0

  ```xml
  <bean id="multipartResolver" class="org.springframework.web.multipart.support.StandardServletMultipartResolver">
  </bean>
  ```

  不同的是在web.xml定义属性

  ```xml
  <servlet>
      <multipart-config>
        <max-file-size>2000000</max-file-size>
      </multipart-config>
  </servlet>
  ```



## 文件下载

```java
package com.fanstudy.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import javax.servlet.http.HttpServletResponse;
import java.io.File;
import java.io.IOException;
import java.io.UnsupportedEncodingException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;

@Controller
@RequestMapping("/download")
public class DownloadController {

    //定义一个文件下载目录
    private static String parentPath = "/home/cf" + File.separator;

    @RequestMapping("/down")
    public String down(HttpServletResponse response){

        //通过输出流写到客户端（浏览器）
        //1.获取文件下载名
        String fileName = "我.jpg";
        //2.通过Path工具类获取一个Path对象
        Path path = Paths.get(parentPath, fileName);

        //3.检查它是否存在,存在则下载
        if(Files.exists(path)){
            //通过response设定响应的类型
            //4.获取文件的后缀
            String fileSuffix = fileName.substring(fileName.lastIndexOf(".") + 1);

            //5.设置contentType，只有指定它才能去下载
            response.setContentType("application/" + fileSuffix);

            //6.添加头信息
            //ISO8859-1
            try {
                response.addHeader("Content-Disposition","attachment; filename=" + new String(fileName.getBytes("UTF-8"), "ISO8859-1"));
            } catch (UnsupportedEncodingException e) {
                e.printStackTrace();
            }
            //7.通过Path写出去
            try {
                Files.copy(path, response.getOutputStream());
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        return "msg";
    }
}
```



## 拦截器

springmvc提供了拦截器，类似于过滤器，它将在我们的请求处理之前先做检查，有权决定接下来是否继续。对我们的请求进行加工。

拦截器可以设置多个

​	通过HandlerInterceptor，这是一个接口

定义了非常重要的三个方法

- 前置处理
- 后置处理
- 完成处理

### 日志

```xml
log4j.rootCategory=INFO, stdout

log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{ABSOLUTE} %5p %t %c{2}:%L - %m%n

log4j.category.org.springframework.beans.factory=DEBUG
```



### 案例一

拦截器实现方法耗时统计与警告

```xml
<!--拦截器的配置-->
<mvc:interceptors>
    <mvc:interceptor>
        <!--
            /*的写法只能拦截一层，不是多层拦截（/user/login）
        -->
        <mvc:mapping path="/**/*"/>
        <bean class="com.fanstudy.interceptors.MethodTimerInterceptor">
        </bean>
    </mvc:interceptor>
</mvc:interceptors>
```



```java
package com.fanstudy.interceptors;

import org.apache.log4j.Logger;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * 方法耗时统计拦截器
 */
public class MethodTimerInterceptor implements HandlerInterceptor {

    private static final Logger LOGGER = Logger.getLogger(MethodTimerInterceptor.class);

    //前置    开始到结束   两个点减法
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //1 定义开始时间
        long start = System.currentTimeMillis();
        //2 将其放到请求域当中
        request.setAttribute("start", start);
        //返回true，才会去找下一个拦截器，如果没有下一个拦截器，则去Controller
        //记录请求日志
        LOGGER.info(request.getRequestURI() + "，请求到达");
        return true;
    }


    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        //1 取出start
        long start = (long) request.getAttribute("start");
        //2 得到end
        long end = System.currentTimeMillis();
        //3 记录一下耗时
        long spendTime = end - start;
        if(spendTime >= 1000){
            LOGGER.warn("方法耗时严重" + spendTime + "毫秒");
        }
        else {
            LOGGER.info("方法耗时正常" + spendTime + "毫秒");
        }
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {

    }
}
```



### 案例二

```xml
<mvc:interceptor>
    <!--
        只想拦截/user/**/*
        还需要开放登录权限
    -->
    <mvc:mapping path="/user/**/*"/>
    <!--排除登录的URI-->
    <mvc:exclude-mapping path="/user/login"/>
    <bean class="com.fanstudy.interceptors.SessionInterceptor">
    </bean>
</mvc:interceptor>
```



```java
package com.fanstudy.interceptors;

import com.fanstudy.pojo.User;
import org.apache.log4j.Logger;
import org.springframework.web.servlet.HandlerInterceptor;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * 回话拦截器
 */
public class SessionInterceptor implements HandlerInterceptor {
    private static final Logger LOGGER = Logger.getLogger(SessionInterceptor.class);

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //检查当前会话是否有User，如果有放行，没有不放行
        Object session_user = request.getSession().getAttribute("SESSION_USER");
        if(session_user == null){
            LOGGER.warn("您不具备权限，请先登录");
            return false;
        }
        else if(session_user instanceof User){
            //再去数据库检查其身份对不对，比如冻结了
            User user = (User) session_user;
            user.setPassword(null);
            request.getSession().setAttribute("SESSION_USER", user);
            LOGGER.info(user.getName() + "正在处于登录状态，可以实行操作");
            return true;
        }
        else {
            LOGGER.warn("请先登录");
            return false;
        }

    }
}
```

### 拦截器执行顺序问题

如果有n个拦截器，并且都能拦截到URI的时候，执行顺序问题。

在springmvc定义的顺序是有关系的，配置在前面的优先拦截。

i1、i2、i3

前置处理i1、i2、i3

后置处理i3、i2、i1



### 拦截器与过滤器的比较

#### 相似

​	都有优先处理请求的权利，可以决定是否将请求转移到实际处理的控制器处，都可以对请求或会话当中的数据进行加工

#### 不同

- 拦截器可以做前置处理，也可以做后置处理，还可以完成后处理，控制更加细一些，过滤器只负责前面的过滤的行为而已。

- 过滤器比拦截器优先执行
- 过滤器是servlet规范里的组件
- 拦截器都是框架自己额外添加的组件

​	

​	

