# java开发规范

## java版本

统一使用jdk 1.8

## 1. 配置文件

- 系统配置(端口号,服务名,配置中心,注册中心相关)统一放在bootstrap.yml文件中
- 业务配置统一放在application.yml文件.建议放在配置中心更好

`注意` bootstrap.yml只能存放[项目介绍](https://devcloud.huaweicloud.com/wiki/project/962471ba833145a1bdd663ae18b995de/wiki/view/doc/318838)中要求的内容。其他的配置一律放在application.yml中

也就是说bootstrap.yml中只能存放如下内容:
```xml
server:
  port: ${端口号}
spring:
  application:
    name: ${服务名}
  cloud:
    config: ##配置中心配置
      failFast: true
      profile: ${环境:sit test uat prod或者自定义}
      name: ${spring.application.name}
      username: admin #配置中心密码
      password: admin
      discovery:
        enabled: true
    consul:
      host: 114.115.161.103
      port: 8500
```
且这些内容必须放在该文件

## 2. maven

#### 2.1、groupId
groupId不能使用顶层坐标`com.chinasofti.futurelab`,顶级坐标归基础架构使用,其他服务严禁使用!每个服务必须自己定义一个.比如用户服务:
```xml
<parent>
  <groupId>com.chinasofti.futurelab</groupId>
  <artifactId>futurelab-base</artifactId>
  <version>3.0-SNAPSHOT</version>
</parent>


<!--看这这里-->
<groupId>com.chinasofti.futurelab.user</groupId> 
<artifactId>base-user</artifactId>
<version>1.0-SNAPSHOT</version>
<packaging>pom</packaging>
```

#### 2.2、关于中台服务依赖问题

1. 顶层pom(项目根目录)

   原则上不依赖任何依赖(除非有api,core-service必须共用的依赖)

   反例:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
       <groupId>com.chinasofti.futurelab.order</groupId>
       <artifactId>base-order</artifactId>
       <version>1.0-SNAPSHOT</version>
       <packaging>pom</packaging>
   
       <parent>
           <groupId>com.chinasofti.futurelab</groupId>
           <artifactId>futurelab-base</artifactId>
           <version>3.0-SNAPSHOT</version>
       </parent>
       <dependencies>
           <dependency>
               <groupId>com.chinasofti.futurelab</groupId>
               <artifactId>web-mvc</artifactId>
               <version>${futurelab-base.version}</version>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-devtools</artifactId>
           </dependency>
           <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
           </dependency>
       </dependencies>
       <modules>
           <module>base-order-api</module>
           <module>base-order-service</module>
       </modules>
   </project>
   ```

   正确做法:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
       <groupId>com.chinasofti.futurelab.order</groupId>
       <artifactId>base-order</artifactId>
       <version>1.0-SNAPSHOT</version>
       <packaging>pom</packaging>
   
       <parent>
           <groupId>com.chinasofti.futurelab</groupId>
           <artifactId>futurelab-base</artifactId>
           <version>2.0-SNAPSHOT</version>
       </parent>
   
       <modules>
           <module>base-order-api</module>
           <module>base-order-service</module>
       </modules>
   </project>
   ```

2. api模块不能依赖`spring-boot-devtools`,而且依赖应该尽量的少,只需要依赖它所需要的。如下:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
       <artifactId>base-order-api</artifactId>
   
       <parent>
           <groupId>com.chinasofti.futurelab.order</groupId>
           <artifactId>base-order</artifactId>
           <version>1.0-SNAPSHOT</version>
       </parent>
   
       <dependencies>
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-openfeign</artifactId>
               <exclusions>
                   <exclusion>
                       <groupId>com.google.guava</groupId>
                       <artifactId>guava</artifactId>
                   </exclusion>
               </exclusions>
           </dependency>
           <dependency>
               <groupId>io.springfox</groupId>
               <artifactId>springfox-swagger2</artifactId>
           </dependency>
       </dependencies>
   </project>
   ```

   > api模块一般只需要feign,swagger即可,因为它需要提供feignclient,和swagger描述

3. core-service模块

   例子:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
   	<modelVersion>4.0.0</modelVersion>
   	<artifactId>base-order-service</artifactId>
       
   	<parent>
   		<groupId>com.chinasofti.futurelab.order</groupId>
   		<artifactId>base-order</artifactId>
   		<version>1.0-SNAPSHOT</version>
   	</parent>
   
   	<dependencies>
   		<dependency>
   			<groupId>${groupId}</groupId>
   			<artifactId>base-order-api</artifactId>
   			<version>${version}</version>
   		</dependency>
   		<dependency>
   			<groupId>com.chinasofti.futurelab</groupId>
   			<artifactId>web-mvc</artifactId>
   			<version>${futurelab-base.version}</version>
   		</dependency>
   		<dependency>
   			<groupId>com.chinasofti.futurelab</groupId>
   			<artifactId>mysql</artifactId>
   			<version>${futurelab-base.version}</version>
   		</dependency>
           <dependency>
               <groupId>org.json</groupId>
               <artifactId>json</artifactId>
   			<version>20180813</version>
           </dependency>
   		<dependency>
   			<groupId>com.alipay.sdk</groupId>
   			<artifactId>alipay-sdk-java</artifactId>
   			<version>3.7.26.ALL</version>
   		</dependency>
       </dependencies>
   	<build>
   		<plugins>
   			<plugin>
   				<groupId>org.springframework.boot</groupId>
   				<artifactId>spring-boot-maven-plugin</artifactId>
   				<executions>
   					<execution>
   						<goals>
   							<goal>repackage</goal>
   						</goals>
   					</execution>
   				</executions>
   			</plugin>
   		</plugins>
   	</build>
   </project>
   ```
