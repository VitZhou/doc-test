# java开发规范

## java版本

统一使用jdk 1.8

## 配置文件

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

## maven

#### groupId
groupId不能使用顶层坐标`com.chinasofti.futurelab`,顶级坐标归基础架构使用,其他服务严禁使用!每个服务必须自己定义一个.比如用户服务:
```xml
<parent>
  <groupId>com.chinasofti.futurelab</groupId>
  <artifactId>futurelab-base</artifactId>
  <version>2.0-SNAPSHOT</version>
</parent>


<!--看这这里-->
<groupId>com.chinasofti.futurelab.user</groupId> 
<artifactId>base-user</artifactId>
<version>1.0-SNAPSHOT</version>
<packaging>pom</packaging>
```

## 包名

每个服务的每个模块的顶级包名需要遵循以下格式:
顶层包名格式为: com.chinasofti.futurelab.{serviceName}.{moduleName}

## .ignore

在根目录添加.ignore文件.内容如下
```java
target/
!.mvn/wrapper/maven-wrapper.jar
*.pdb

### STS ###
.apt_generated
.classpath
.factorypath
.project
.settings
.springBeans

### IntelliJ IDEA ###
.idea
.mvn
*.iws
*.iml
*.ipr

### NetBeans ###
nbproject/private/
build/
nbbuild/
dist/
nbdist/
.nb-gradle/

/logs
```