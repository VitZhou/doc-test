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
               <groupId>com.chinasofti.futurelab</groupId>
               <artifactId>core</artifactId>
               <version>${futurelab-base.version}</version>
           </dependency>
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


## 3. 日志打印

日志打印必须是能帮助你定位问题的,必须将有意义的参数信息打印出来,如果是捕获了异常信息,则必须打印堆栈信息.

1. 一般日志打印

    ```java
     log.info("数据同步执行完毕, 更新:{}, 新增:{}, 新增开关:{}", updateCount, addCount, alarmStatusCount);
    ```
    日志尽量使用占位符`{}`,使用了它,sl4j会调用对应位置的参数的toString方法(所以如果没有toString方法的参数就不建议再使用占位符了)。

2. 堆栈日志打印:
  
    反面例子1:
    
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
    
    反面例子2:
        
    ```java
    catch (Exception e) {
        e.printStackTrace();
    } 
    ```
    异常信息并不会输出到日志文件中
    
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

3. 如果输出的对象没有覆盖toString方法

    如果输出的对象没有重写toString方法的话,除了error级别的日志,都需要先判断日志级别是否已打开.从而提高性能
    
    ```java
    if (logger.isInfoEnabled()){
       logger.info("aaaa,param:{}",param);
    }
    ```
#### 敏感信息.

1. 日志中原则上不允许输出敏感信息(如手机号,身份证号),如果要输出需要加密.
2. 敏感信息不能因为后台报错而将其通过异常传递到前端.
  
   反例:
    ```java
    try {
        return localeProvincesDao.selectPage(page, queryWrapper);
    } catch (Exception e) {
        logger.error("分页查询失败,entity=" + queryWrapper.getEntity(), e);
        throw new DbOperationException("分页查询失败,省份id="+id);
    }
    ```
    你可以只抛出异常,异常中不携带任何信息,而通过日志记录当时发生的问题,如:
    ```java
    try {
        return localeProvincesDao.selectPage(page, queryWrapper);
    } catch (Exception e) {
        logger.error("查询省份信息失败,id=" + id+",xxx="+xxx, e);
        throw new DbOperationException();
    }
    ```
   
## 4. 关于循环调用

不允许循环调用接口,这样做法效率太低下,导致最终接口响应时间太长,甚至超时

反例

```java
for(...){
    restTemplate.get(uri)....
}
```

或

```java
for(...){
   baseBuilder.execute(restRequest); 
}
```

解决方案.如果聚合服务没法一次性调用中台服务的接口,则可以要求中台服务提供批量接口. 通过批量接口一次性将参数传入. 如果需要同时调用多个服务的接口建议使用多线程异步调用的方式.具体请看[这里](../../java/utils/RestUriBuilder.md)

> 注意这里只是举例,线程池的具体参数要根据业务而定,而不是照抄.尤其是线程池的大小.且必须给线程定义名称,方便定位问题


### 5. 多线程使用

不要直接在方法中new线程进行操作，应通过线程池操作

反例

```
Thread thread = new Thread(new Runnable() {
            @Override
            public void run() {
                //do something...
            }
        });
thread.start();
```

应调整为线程池操作

```
@Configuration
public class ThreadPoolConfiguration implements InitializingBean, DisposableBean {

    private ExecutorService executorService = null;

    @Override
    public void destroy() throws Exception {
        if (executorService != null) {
            executorService.shutdown();
        }
    }

    @Override
    public void afterPropertiesSet() throws Exception {
        //Executors含多种构造方法，选择合适构造方式
        executorService = Executors.newFixedThreadPool(2,
                        new ThreadFactoryBuilder().setNameFormat("do-somethin-%d").build());
    }
}

public SomeTask implements Runnable {
    @Override
    public void run() {
        //do something...
    }
    
}
```

```
@Autowired
private ExecutorService executorService



SomeTask task = new SomeTask();
executorService.execute(task);

```

### 6. 常量定义
代码中不应使用硬编码，而是通过定义常量来使用

反例
```
 String result = map.get("abc");
```

应调整为
```
//如果多个类均使用到下面常量，应提取到公共常量类中
private static final String ABC = "abc";
···
String result = map.get(ABC);

```
