# maven

pom：project object model

下载、解压	https://maven.apache.org/download.cgi

修改 conf/settings.xml	localRepository	mirror

```
<groupId>com.fanstudy</groupId>		一般为公司域名倒写
<artifactId>helloweb</artifactId>	项目、模块名称
<version>1.0-SNAPSHOT</version>		版本号
```



```
<properties>	属性定义
  <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  <maven.compiler.source>1.8</maven.compiler.source>	编译源代码的版本
  <maven.compiler.target>1.8</maven.compiler.target>
</properties>
```



```
<dependency>	要什么jar中央仓库搜索即可	通过坐标描述
  <groupId>junit</groupId>
  <artifactId>junit</artifactId>
  <version>4.12</version>
  <scope>test</scope>
</dependency>
```



```
<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.38</version>
</dependency>
```



```
<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.4.6</version>
</dependency>
```

