# mysql模块

这个模块提供mysql的连接池以及mybatis,sharding-jdbc,pageHelper等依赖.

## mysql依赖

要使用mysql,需要单独添加其以来:

```xml
<dependency>
    <groupId>com.chinasofti.futurelab</groupId>
    <artifactId>mysql</artifactId>
    <version>${futurelab-base.version}</version>
</dependency>
```


## MySqlConfiguration

这个configuration类中添加了:
```java
@MapperScan(basePackages = {"com.chinasofti.**.dao.**"})
```
所以mybatis会默认扫描com.chinasofti.**.dao路径下的类. configuration类提供了一些默认连接池参数,详情请查看源码

## mybatis-config.xml

需要在resource目录下创建该文件,并添加以下内容:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <settings>
        <setting name="mapUnderscoreToCamelCase" value="true"/>
    </settings>
    <typeHandlers>
        <typeHandler handler="org.apache.ibatis.type.InstantTypeHandler" />
        <typeHandler handler="org.apache.ibatis.type.LocalDateTimeTypeHandler" />
        <typeHandler handler="org.apache.ibatis.type.LocalDateTypeHandler" />
        <typeHandler handler="org.apache.ibatis.type.LocalTimeTypeHandler" />
        <typeHandler handler="org.apache.ibatis.type.OffsetDateTimeTypeHandler" />
        <typeHandler handler="org.apache.ibatis.type.OffsetTimeTypeHandler" />
        <typeHandler handler="org.apache.ibatis.type.ZonedDateTimeTypeHandler" />
    </typeHandlers>
</configuration>
```

## sharding-jdbc配置

我们默认使用读写分离(暂时不使用分表分库).所以需要添加读写分配的配置.

将以下配置添加到配置中心中:

```yaml
spring:
  shardingsphere:
    datasource:
      names: master,slave
      master:
        type: com.zaxxer.hikari.HikariDataSource
        driverClassName: com.mysql.jdbc.Driver
        jdbcUrl: jdbc:mysql://localhost:3306/future_demo?useSSL=false&characterEncoding=utf8&statementInterceptors=brave.mysql.TracingStatementInterceptor
        username: root
        password: root
      slave:
        type: com.zaxxer.hikari.HikariDataSource
        driverClassName: com.mysql.jdbc.Driver
        jdbcUrl: jdbc:mysql://localhost:3306/future_demo?useSSL=false&characterEncoding=utf8&statementInterceptors=brave.mysql.TracingStatementInterceptor
        username: root
        password: root
    masterslave:
      name: ms
      master-data-source-name: master #指定master数据源名.跟上面的`names`参数对应
      slave-data-source-names: slave #指定slave数据源名.跟上面的`names`参数对应
    props:
      sql:
        show: true #是否显示sql.建议只在dev,sit环境使用
```
> 数据库连接池配置参考[数据库连接池配置](java/config/数据库连接池.md)

> 更多sharding-jdbc的知识请查看其[官网](https://shardingsphere.apache.org),jdbcUrl是Hikari的参数.并不是sharding-jdbc参数

> &statementInterceptors=brave.mysql.TracingStatementInterceptor参数不能少.否则zipkin无法拦截mysql操作


只需要将其添加到maven依赖,然后就可按照sharding-jdbc[官网](https://shardingsphere.apache.org)中所述一样使用.在[快速入门](/java/getting_started/mysql.md)中已有例子介绍.具体细节请查看sharding-jdbc官网.

## 为何从mybatis-plus退回mybatis

Mybatis-plus的好处:

1. 大大提高了开发效率,因为简单的crud不需要去写xml文件
2. 灵活的api,开箱即用

它的坏处:

1. 它的序列化方案使用的java的序列化,性能无需多说
2. 我们团队没有人十分熟悉它(上uat已带来了一次因为配置问题而导致的长时间Block)
3. 正因为它太过灵活,所以业务的代码非常随意,失去了应有的分层结构(controller,service,resource,dao)。
4. 不想写xml文件有备选方案,可以使用idea的插件来解决

综上所述,在弊大于利的情况下,经过慎重考虑回退到了mabtis.

>不允许使用其它的依赖(例如BaseMapper).他们跟mybatis-plus有着类似的问题.mybatis-plus之类的只适合企业内部等小型系统.不适合做互联网系统


### mysql主从部署

看[这篇博客](https://www.jianshu.com/p/b0cf461451fb)