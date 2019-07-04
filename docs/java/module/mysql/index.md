# Mysql模块

该模块集成了mybatis,sharding-jdbc,pageHelper.

只需要将其添加到maven依赖,并`@Import(MyBatisDataSourceConfiguration.class)`.然后就可按照sharding-jdbc[官网](https://shardingsphere.apache.org)中所述一样使用.在[快速入门](/java/getting_started/mysql.md)中已有例子介绍.具体细节请查看sharding-jdbc官网.

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

## 配置

在配置中心加如下配置:

```yaml
spring:
  shardingsphere:
    datasource:
      names: master,slave
      master:
        type: com.zaxxer.hikari.HikariDataSource
        driverClassName: com.mysql.jdbc.Driver
        jdbcUrl: jdbc:mysql://localhost:3306/future_demo?useSSL=false&characterEncoding=utf8 
        username: root
        password: root
      slave:
        type: com.zaxxer.hikari.HikariDataSource
        jdbcUrl: jdbc:mysql://localhost:33066/future_demo?useSSL=false&characterEncoding=utf8
        username: root
        password: root
        maximumPoolSize: 50 #最大连接数,可不填base已经设置该值
        maxLifetime: 300000 #最大空闲数,可不填base已经设置该值
        driver-class-name: com.mysql.jdbc.Driver
```

> 注意jdbcUrl是HikariDataSource需要的参数

