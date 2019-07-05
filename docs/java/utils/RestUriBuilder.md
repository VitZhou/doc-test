# RestUriBuilder工具

该工具主要是简化RestTemplate的代码量,以及解决RestTemplate无法深层次泛型嵌套的问题.主要有两个类:**RestUriBaseBuilder**和**RestUriPageBuilder**.

聚合服务请求中台服务的的开发建议使用这些工具进行远程调用。如果是聚合服务调用其他的外部uri不强制使用该工具。

## 1、配置

在任意配置文件中加入以下配置:

```yaml
app:
  gateway:
    uri: localhost:8787
```

> dev环境不需要自己添加配置中心已经有

## 2、使用

使用方式跟RestTemplate差不多.如下:

1. 返回值是个对象使用RestUriBaseBuilder

    ```java
    @Autowired
    private RestUriBaseBuilder baseBuilder;

    public ResponseObject<String> example(@PathVariable String name) {
        RestRequest<String> restRequest = new RestRequest.Builder<String>()
                .withClazz(String.class)
                .withServiceName("exampleService")
                .withUri("example/2/" + name)
                .withMethod(HttpMethod.GET)
                .build();
        Optional<ResponseObject<String>> optional = baseBuilder.execute(restRequest);
        return optional.get();
    }
    ```

1. 如果返回值是个集合类型则使用RestUriPageBuilder

    ```java
    @Autowired
    private RestUriPageBuilder builder;

    public PageResponseHelper<String> example(@PathVariable String name){
        RestRequest<String> restRequest = new RestRequest.Builder<String>()
                .withClazz(String.class)
                .withServiceName("exampleService")
                .withUri("example/" + name)
                .withMethod(HttpMethod.GET)
                .build();
        Optional<PageResponseHelper<String>> optional = builder.execute(restRequest);
        return optional.orElseGet(() -> PageResponseHelper.internalError("cuole"));
    }
    ```

## 3、参数说明

RestRequest一共含有如下参数:

- serviceName: 需要请求的服务的服务名,必填
- uri: 请求路径,必填
- clazz: 返回体对象,必填
- method: 请求方式(例如:get,post等),必填
- headers: 请求头,选填
- uriVariables: uri参数,选填(不会使用的话建议不要轻易使用)
- body: request body参数,选填。如果填写了会使用json方式传递

### 关于请求路径的说明

如果你请求的完整路径是: **http://localhost:8787/demoService/exmapleUri/param**。其中localhost:8787为gateway地址.demoService为服务名.exmapleUri/param为uri.且有个request body参数为bodyParam。则请求参数如下:

```java
RestRequest<String> restRequest = new RestRequest.Builder<String>()
                .withClazz(String.class)
                .withServiceName("demoService")
                .withUri("exmapleUri/param")
                .withMethod(HttpMethod.GET)
                .withBody(bodyParam) //注意,bodyParam是对象.而不是hashMap
                .build();
```

## 4、关于字段不一致的问题

此问题只会存在聚合服务.

假设聚合服务暴露给前端的字段为`name`,而中台服务返回的字段为`id`.这时候因为字段不一样,导致json反序列化的时候没法给`name`复制.此时可以通过在dto对象的`name`属性中添加`@JsonAlias`注解来解决.例如:

```java
public class Person{
    @JsonAlias('id')
    private String name;
}
```

