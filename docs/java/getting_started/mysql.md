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

## main

```java
@Import({BaseConfiguration.class, DiscoveryClientConfiguration.class, WebApplication.class,
        MyBatisDataSourceConfiguration.class})
@SpringBootApplication
public class ServerApplication {

    public static void main(String[] args) {
        SpringApplication.run(ServerApplication.class, args);
    }
}
```

## MyBatisDataSourceConfiguration

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
        type: com.alibaba.druid.pool.DruidDataSource
        driver-class-name: com.mysql.jdbc.Driver
        url: jdbc:mysql://localhost:3306/future_demo?useSSL=false&characterEncoding=utf8 #mysql连接信息
        username: root
        password: root
      slave:
        type: com.alibaba.druid.pool.DruidDataSource
        driver-class-name: com.mysql.jdbc.Driver
        url: jdbc:mysql://localhost:33066/future_demo?useSSL=false&characterEncoding=utf8
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

> 更多sharding-jdbc的知识请查看其[官网](https://shardingsphere.apache.org)

