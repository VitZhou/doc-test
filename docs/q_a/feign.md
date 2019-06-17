# feign相关问题


## feign client不存在
	
#### 问题描述: feign client不存在
![](http://ww2.sinaimg.cn/large/006tNc79gy1g3baez6ricj314v0h6myb.jpg)

异常信息:
```console
Description:

Field test in com.chinasofti.futurelab.evaluate.modules.resource.QuizAnswerRecordResource required a bean of type 'com.chinasofti.futurelab.base.order.api.client.MyTest' that could not be found.

The injection point has the following annotations:
- @org.springframework.beans.factory.annotation.Autowired(required=true)
```

#### 解决方案

将包名client改为clients

#### 原因

因为futurebase指定了feign的扫描包为clients,如下:
```java
@EnableDiscoveryClient
@EnableFeignClients(
    value = {"com.chinasofti.**.service", "com.chinasofti.**.clients"},
    defaultConfiguration = {FeignCustomConfiguration.class}
)
public class DiscoveryClientConfiguration {
```
    