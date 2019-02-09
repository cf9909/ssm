## mybatis plugin安装

打开 idea.vmoptions (Help ->Edit Custom VM Options...)

添加

```
-XX:+DisableAttachMechanism
-javaagent:E:\tools\idea\MybatisPlugin\ideaagent-1.2.jar
```

```xml
<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <version>5.1.38</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>3.4.6</version>
</dependency>
```



## 配置文件

### properties

```
dataSource里面字面值属性优先级》外部properties文件里面的属性的优先级》properties里面子节点的属性

jdbc.properties

url=jdbc:mysql://localhost:3306/mybatis-plus?useUnicode=true&characterEncoding=utf-8&zeroDateTimeBehavior=convertToNull&transformedBitIsBoolean=true&useSSL=false
driver=com.mysql.jdbc.Driver
username=root
password=123456

<properties resource="jdbc.properties">
    <!--<property name="username" value="123456"/>-->
</properties>
```



### settings

默认配置一般是友好的

```xml
<!--
    开启下划线风格转变为驼峰风格
    数据库设计采用下划线风格    last_name
    javabean采用驼峰风格      lastName
    不匹配的问题
-->

<!-- Enables automatic mapping from classic database column names A_COLUMN to camel case classic Java property names aColumn -->
<setting name="mapUnderscoreToCamelCase" value="true"/>
```







### typeAliases

```xml
<!--类型别名-->
<!--com.fanstudy.xxx    xxx-->
<typeAliases>
    <!--注册一个简写的类名，可以在其它的mapper文件中被引用-->
    <!--不推荐使用-->
    <typeAlias type="com.fanstudy.pojo.Girl" alias="girl"/>
    
    <!--直接注册整个包，该包下所有类都生效，默认规则为简写类型-->
    <package name="com.fanstudy.pojo"/>
</typeAliases>
```



### typeHandlers

一般情况下不需要额外添加，特殊情况再考虑



### enenvironments

```xml
<environments default="dev">
    <environment id="dev">
        <transactionManager type="JDBC"></transactionManager>
        <dataSource type="UNPOOLED">
            <property name="url" value="${url}"/>
            <property name="driver" value="${driver}"/>
            <!--可以通过${}引用properties里面规定的值-->
            <property name="username" value="${username}"/>
            <property name="password" value="${password}}"/>
        </dataSource>
    </environment>
</environments>
```



### mappers

```xml
<mappers>
    <!--第一种：通过类路径方式引入XML文件-->
    <!--不要写.要写/-->
    <mapper resource="com/fanstudy/mapper/GirlMapper.xml"></mapper>
    
    <!--第二种：通过URL   协议：地址的方式引入-->
    <mapper url="file:///E:/project/hellomvc/src/main/resources/com.fanstudy.mapper/GirlMapper.xml"/>
	<!--第三种：通过接口的全限定名引入，必须保持我们的接口和Mapper.xml在同包之下-->
    <mapper class="com.fanstudy.mapper.GirlMapper"/>
    
    <!--第四种：引入一个包的方式，以后只要是在这个包下新建的mapper，不需要重新引入-->
    <package name="com.fanstudy.mapper"/>
</mappers>
```



## mybatis参数问题

### 单个基本或非基本数据类型入参问题

如果仅仅是简单的一个单值传入，那么#{}表达式里面随便写什么都可以，只有一个参数，mybatis没有入参绑定的烦恼，建议还是要写有含义的名称。



关于

Unknown column 'name' in 'where clause'

问题的解决，肯定是SQL语句的列名写错导致。



关于

Expected one result (or null) to be returned by selectOne(), but found: 2

问题的解决

我们所要求的结果为一个对象，而查询返回为两个，这个时候不匹配

只要不是按主键查询，结果可能不止一个，



### 多个基本或非基本数据类型入参问题

String



关于

Cause: org.apache.ibatis.binding.BindingException: Parameter 'name' not found. Available parameters are [arg1, arg0, param1, param2]

问题的解决

某个参数没找到，可用的参数仅仅是

arg0, arg1

param1, param2

```xml
<!--使用默认参数名称风格，非常不友好，这种方式是不可取的-->
<select id="queryByNameFlower" resultType="girl">
    <!--select * from girl where last_name = #{param1} and flower = #{param2}-->
    select * from girl where last_name = #{arg0} and flower = #{arg1}
</select>
```



推荐使用

```java
Mapper文件
/**
 * 加上一个注解
 * @Param
 * @param name
 * @param flower
 * @return
 */
Girl queryByNameFlower(@Param("name") String name, @Param("flower") String flower);
```



```xml
<select id="queryByNameFlower" resultType="girl">
    select * from girl where last_name = #{name} and flower = #{flower}
</select>
```





### 单个Javabean

```java
Girl queryByNameFlower2(Girl girl);
```

```xml
<select id="queryByNameFlower2" resultType="girl">
    select * from girl where last_name = #{lastName} and flower = #{flower}
</select>
```

默认通过Javabean里面的属性的名称去引用，通过getter方法去找这些值。

提供了get set方法的就叫做属性



### Map

```java
Girl queryByNameFlower3(Map<String, Object> map);
```

```xml
<select id="queryByNameFlower3" resultType="girl">
    select * from girl where last_name = #{lastName} and flower = #{flower}
</select>
```



按照这种方式封装，就是按照键的名称进行取值





### 多个Javabean

```java
Girl queryByAB(@Param("a") A a, @Param("b") B b);
```



```xml
<select id="queryByAB" resultType="girl">
    select * from girl where last_name = #{a.name} and flower = #{b.flower}
</select>
```



### 一组值的传入（List集合的问题）





## 结果集的封装





## 动态SQL



### 添加日志

```xml
<!-- https://mvnrepository.com/artifact/log4j/log4j -->
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
```


```xml
<setting name="logImpl" value="LOG4J"/>
```

log4j.properties

```
# Global logging configuration
log4j.rootLogger=ERROR, stdout
# MyBatis logging configuration...
log4j.logger.com.fanstudy.mapper=DEBUG
# Console output...
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```



### if

### where

```xml
<!--mybatis 使用where标签可以完美解决
    可以自动消除多余的and

    if希望处理这个字符串不为null，""
    1.如何与空字符串比较
    2.如何连接多个条件
-->
<select id="queryByCountryCity" resultType="com.fanstudy.pojo.Addresses">
    select * from addresses
    <where>
        <if test="country != null and country != ''">
            and country = #{country}
        </if>
        <if test="city != null">
            and city = #{city}
        </if>
    </where>
</select>
```

### set

```xml
<!--
    功能：
    根据传入对象动态修改其中的值
    如果某个字段传入的非空值，再去修改，否则不修改

    where后面条件使用addr_id作为条件

    使用set标签
    可以自动消除多余的后置的,
-->
<update id="update" parameterType="com.fanstudy.pojo.Addresses">
    update addresses
    <set>
        <if test="city != null and city != ''">
            city = #{city},
        </if>
        <if test="country != null and country != ''">
            country = #{country},
        </if>
        <if test="street != null and street != ''">
            street = #{street},
        </if>
        <if test="state != null and state != ''">
            state = #{state},
        </if>
        <if test="zip != null and zip != ''">
            zip = #{zip}
        </if>
    </set>
    <where>
        addr_id = #{addrId}
    </where>
</update>
```

### trim

```xml
<select id="queryTrim" resultType="com.fanstudy.pojo.Addresses" parameterType="com.fanstudy.pojo.Addresses">
    select * from addresses
    <trim prefix="WHERE" suffixOverrides="AND">
        <if test="city != null and city != ''">
            city = #{city} and
        </if>
        <if test="country != null and country != ''">
            country = #{country} and
        </if>
        <if test="street != null and street != ''">
            street = #{street} and
        </if>
        <if test="state != null and state != ''">
            state = #{state} and
        </if>
        <if test="zip != null and zip != ''">
            zip = #{zip} and
        </if>
    </trim>
</select>
```

### foreach

```xml
<!--
    foreach
    collection 描述集合list，set，map
    open：后面SQL语句的拼接以什么开头
    close：以什么结尾
    item：是数据项的代号
    separator：item之间的分隔符
    index：如果需要使用标号，也可以使用
-->
<select id="queryByIds" resultType="com.fanstudy.pojo.Addresses">
    select * from addresses
    <where>
        addr_id in
        <foreach collection="list" open="(" close=")" item="item" separator="," index="index">
            #{item} + #{index}
        </foreach>
    </where>
</select>
```







```xml
【模糊查询解决方案一】在应用程序层面加入%%拼接
List<Addresses> addresses = mapper.queryLike("%Law%");

<!--#{}不支持%%拼接-->
<select id="queryLike" resultType="addresses">
    select * from addresses
    where
    city like #{city}
</select>
    
【模糊查询解决方案二】通过mysql函数concat完成
List<Addresses> addresses = mapper.queryLike("Law");
    
<select id="queryLike" resultType="addresses">
    select * from addresses
    where
    city like concat('%', #{city}, '%')
</select>
    
【模糊查询解决方案三】通过bind标签对我们的变量进行重新绑定，然后用新绑定的变量进行引用即可
List<Addresses> addresses = mapper.queryLike("Law");
    
<select id="queryLike" resultType="addresses">
    <bind name="_city" value="'%'+city+'%'"/>
    select * from addresses
    where
    city like #{_city}
</select>
```



### sql

```xml
<!--将最常用的列，抽取成为一个SQL片段，目的是被别人引用-->
<sql id="baseColumn">
    country,state,city
</sql>

<select id="listAll" resultType="addresses">
    select
    <include refid="baseColumn"/>
    from addresses
</select>
```



## XML映射文件

### 缓存

```java
@Test
public void m7(){
    SqlSession sqlSession = MybatisUtil.getSession();

    AddressesMapper mapper = sqlSession.getMapper(AddressesMapper.class);

    List<Addresses> addresses = mapper.listAll();
    System.out.println(addresses);

    List<Addresses> addresses2 = mapper.listAll();
    System.out.println(addresses2);
    sqlSession.close();
}

//select一次
```





```java
@Test
public void m7(){
    SqlSession sqlSession = MybatisUtil.getSession();

    AddressesMapper mapper = sqlSession.getMapper(AddressesMapper.class);

    List<Addresses> addresses = mapper.listAll();
    System.out.println(addresses);
    
    sqlSession.close();
    /**
     * 一级缓存是会话级别的（要在同一会话当中才有效）
     * 如果开启了二级缓存，先去二级缓存中尝试命中
     * 如果无法命中，尝试去一级缓存中命中
     * 如果仍无法命中，再去数据库查询
     */
    sqlSession = MybatisUtil.getSession();
    mapper = sqlSession.getMapper(AddressesMapper.class);
    List<Addresses> addresses2 = mapper.listAll();
    System.out.println(addresses2);
    sqlSession.close();
}

//select两次
```



```java
@Test
public void m8(){
    SqlSession sqlSession = MybatisUtil.getSession();

    AddressesMapper mapper = sqlSession.getMapper(AddressesMapper.class);

    List<Addresses> addresses = mapper.listAll();
    //【缓存失效方式一】查询之后进行了增删改的行为
    Addresses a = new Addresses();
    a.setAddrId(5);
    a.setCountry("中国");
    a.setCity("aa");
    a.setState("湖北");
    a.setStreet("某某某街道");
    mapper.update(a);

    List<Addresses> addresses2 = mapper.listAll();
    System.out.println(addresses2);

    sqlSession.close();
}

//select两次
```



```java
@Test
public void m9(){
    SqlSession sqlSession = MybatisUtil.getSession();

    AddressesMapper mapper = sqlSession.getMapper(AddressesMapper.class);

    List<Addresses> addresses = mapper.listAll();
    //【缓存失效方式二】强制清空缓存，清空所有
    sqlSession.clearCache();

    List<Addresses> addresses2 = mapper.listAll();
    System.out.println(addresses2);

    sqlSession.close();
}

//select两次
```



### 二级缓存

```xml
<!--默认值，不确定，设置为true-->
<setting name="cacheEnabled" value="true"/>
```

mapper.xml

```xml
<!--开启二级缓存-->
<cache/>
```



关于org.apache.ibatis.cache.CacheException: Error serializing object.  Cause: java.io.NotSerializableException: com.fanstudy.pojo.Addresses问题的解决

```java
public class Addresses implements Serializable
```



```java
@Test
public void m10(){
    SqlSession sqlSession = MybatisUtil.getSession();
    SqlSession sqlSession2 = MybatisUtil.getSession();

    AddressesMapper mapper = sqlSession.getMapper(AddressesMapper.class);
    AddressesMapper mapper2 = sqlSession2.getMapper(AddressesMapper.class);

    List<Addresses> addresses = mapper.listAll();
    System.out.println(addresses);
    sqlSession.close();

    List<Addresses> addresses2 = mapper2.listAll();
    System.out.println(addresses2);
    sqlSession2.close();
}

//select一次
```



## 实体关系问题

### 从数量关系上进行描述

单方考虑

1	1	强弱

1	n

```xml
<!--
mybatis对于处理简单的单表查询使用resultType就可以解决
对于多表联合查询往往需要使用resultMap进行详细的描述
告诉mybatis怎么封装，规矩由我们定
-->
<!--封装方式一：官方推荐使用-->
<resultMap id="userWithDetailMap" type="com.fanstudy.pojo.UserWithDetail">
    <!--User基本信息-->
    <!--property javabean的属性    column 数据库里面查询出来的列-->
    <id property="id" column="uid"/>
    <result property="phone" column="phone"/>
    <result property="password" column="password"/>
    <result property="createDate" column="create_date"/>
    <result property="status" column="status"/>
    <!--持有UserDatil，这个如何封装
    -->
    <association property="userDetail" javaType="com.fanstudy.pojo.UserDetail">
        <id property="id" column="udId"/>
        <result property="address" column="address"/>
        <result property="cid" column="cid"/>
    </association>
</resultMap>


<!--封装方式二-->
<resultMap id="userWithDetailMap2" type="com.fanstudy.pojo.UserWithDetail">
    <!--User基本信息-->
    <id property="id" column="uid"/>
    <result property="phone" column="phone"/>
    <result property="password" column="password"/>
    <result property="createDate" column="create_date"/>
    <result property="status" column="status"/>
    <!--连缀的写法-->
    <result property="userDetail.id" column="udId"/>
    <result property="userDetail.address" column="address"/>
    <result property="userDetail.cid" column="cid"/>
</resultMap>

<select id="queryById" resultMap="userWithDetailMap">
    select t1.id as uid, t1.phone, t1.password, t1.create_date, t1.status, t2.id as udId, t2.address, t2.cid
    from user t1, user_detail t2
    <where>
        t1.id = t2.u_id
        and t1.id = #{id}
    </where>
</select>
```



```xml
<!--封装方式三
    支持分步查询
    如果有的查询太复杂了，推荐使用分步查询
-->
<resultMap id="userBaseMap" type="com.fanstudy.pojo.UserWithDetail">
    <!--User基本信息-->
    <id property="id" column="uid"/>
    <result property="phone" column="phone"/>
    <result property="password" column="password"/>
    <result property="createDate" column="create_date"/>
    <result property="status" column="status"/>
</resultMap>

UserDetailMapper.xml
<mapper namespace="com.fanstudy.mapper.UserDetailMapper">
    <select id="queryByUserId" resultType="com.fanstudy.pojo.UserDetail">
        select * from user_detail
        where u_id = #{id}
    </select>
</mapper>

<resultMap id="userWithDetailMap3" extends="userBaseMap" type="com.fanstudy.pojo.UserWithDetail">
    <association property="userDetail" select="com.fanstudy.mapper.UserDetailMapper.queryByUserId" column="uid">
    </association>
</resultMap>

<select id="queryByIdByStep" resultMap="userWithDetailMap3">
    select t1.id as uid, t1.phone, t1.password, t1.create_date, t1.status
    from user t1
    <where>
        and t1.id = #{id}
    </where>
</select>
```





```xml
<resultMap id="userWithBlog" extends="userBaseMap" type="com.fanstudy.pojo.UserBlog">
    <!--封装博客-->
    <collection property="blog" ofType="com.fanstudy.pojo.Blog">
        <id property="id" column="bid"/>
        <result property="title" column="title"/>
        <result property="summary" column="summary"/>
        <result property="content" column="blogContent"/>
        <!--博客里面还有评论-->
        <collection property="comments" ofType="com.fanstudy.pojo.Comment">
            <id property="id" column="cid"/>
            <result property="content" column="commentContent"/>
        </collection>
    </collection>
</resultMap>
```

association：

collection：一对多



```xml
<select id="queryByIdWithBlog" resultMap="userWithBlog">
    select t1.id as uid, t1.phone, t1.password, t1.create_date, t1.status,
    t2.id as bid, t2.title, t2.summary, t2.content as blogContent,
    t3.id cid, t3.content as commentContent
    from user t1, blog t2, comment t3
    <where>
        t1.id = t2.u_id
        and t2.id = t3.b_id
        and t1.id = #{id}
    </where>
</select>
```

