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

## 日志打印

日志打印必须是能帮助你定位问题的,必须将有意义的参数信息打印出来,如果是捕获了异常信息,则必须打印堆栈信息.

1. 一般日志打印

    ```java
     log.info("数据同步执行完毕, 更新:{}, 新增:{}, 新增开关:{}", updateCount, addCount, alarmStatusCount);
    ```
    日志尽量使用占位符`{}`,使用了它,sl4j会调用对应位置的参数的toString方法(所以如果没有toString方法的参数就不建议再使用占位符了)。

2. 堆栈日志打印:
    
    反面例子:
    
    ```java
    catch (KeyManagementException e) {
        logger.error("-------KeyManagemen{}", e.getMessage());
    } catch (KeyStoreException e) {
        logger.error("-------KeyStoreException{}", e.getMessage());
    } catch (IOException e) {
        logger.error("-------IOException{}", e.getMessage());
    }
    ```
    这种日志对于生产对位没有任何帮助.
    
    应该调整为如下:
    
    ```java
    catch (KeyManagementException e) {
        logger.error("key管理出现未知异常,appKey="+appKey+",appSecret="+appSecret, e);
    } catch (KeyStoreException e) {
        logger.error("key存储失败,appKey="+appKey+",appSecret="+appSecret, e);
    } catch (IOException e) {
        logger.error("调用短信平台失败,appKey="+appKey+",appSecret="+appSecret, e);
    }
    ```
    > 注意要打印堆栈信息的话就无法使用占位符`{}`. 如果少量参数的话可以使用字符串拼接,如果参数较多则使用StringBuilder来拼接字符串
    
    