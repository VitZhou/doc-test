# data-source模块

这个模块提供mysql的连接池以及mybatis依赖. 注意,我们使用的是mybatis-plus依赖.

## data-source依赖

```xml
<dependency>
    <groupId>com.chinasofti.futurelab</groupId>
    <artifactId>data-source</artifactId>
    <version>${project.version}</version>
</dependency>
```

> web-mvc中已有这个依赖

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

## 关于一些默认参数说明

mybatis-plus的有许多参数,建议使用之前先查看其[文档](https://baomidou.gitee.io/mybatis-plus-doc/#/quick-start)

plus可以控制id的生成策略,详情看[这里](https://baomidou.gitee.io/mybatis-plus-doc/#/logic-delete?id=spring-boot-mp-starter%e9%85%8d%e7%bd%ae%e5%8f%82%e8%80%83)
我们默认使用的自增(历史原因),但是新功能严禁使用自增.所以需要再各自的服务里面设置不同的id生成策略

## 关于分布式id

1. 由于mybatis已经自带了分布式id工具`IdWorker`.所以不再单独提供分布式id. 要想使用分布式id,只需要将其`id-type`配置为`ID_WORKER`或者`ID_WORKER_STR`即可.
2. `IdWorker`分别提供getId(),getIdStr(),getTimeId(). 配置参数`ID_WORKER`和`ID_WORKER_STR`分别对应getId(),getIdStr().所以如果是订单或者其他需要有意义的编号则需要手工调用其getTimeId()方法.如下:
    ```java
       entity.setId(IdWorker.getTimeId());
    ```
    > 如果entity的id中已有值,mybatis-plus就不会再设置值.


