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

