# RestTemplate

base的core模块提供了四个restTemplate(列出的是bean名称):
- restTemplate: 优先级最高的restTemplate,供feign使用(业务人员无需手工配置)
- rawRestTemplate: 用于聚合服务调用中台服务
- thirdPartyRestTemplate: 用于调用第三方的接口
- ignoreSSLRestTemplate: 用于请求Https的uri(注意一旦使用这个则会忽略证书校验,请求内容是透明的.如果安全性较高的请求不要使用,使用一定要注意使用场景！！！！！)

#### 使用方式

1. restTemplate

```java
@Qualifier(Constants.RAW_REST_TEMPLATE)
@Autowired
private RestTemplate restTemplate;
```

2. thirdPartyRestTemplate

```java
@Qualifier(Constants.THIRD_PARTY_REST_TEMPLATE)
@Autowired
private RestTemplate restTemplate;
```

3. ignoreSSLRestTemplate

```java
@Qualifier(Constants.IGNORE_SSL_REST_TEMPLATE)
@Autowired
private RestTemplate restTemplate;
```
