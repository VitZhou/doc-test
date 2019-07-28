# RestTemplate配置

## 超时

future-base 提供restTemplate支持url级的超时设置.例如:

```yaml
app:
  http:
    hosts:
      - host: baidu.com #host name。域名或者ip:port  注意不需要写http://或者https://
        urls:
          - url: /empty/2/{name}
            method: GET
            connectTimeout: 2000 #单位:毫秒
            readTimeout: 3000 #单位:毫秒
            writeTimeout: 4000 #单位:毫秒
```

> 注意.每配置一个url都会单独创建个连接池.所以如果不是必要的url不建议单独设置超时.尽量让连接池共用,复用连接能节省服务器资源开销

#### RestUriBuilder

RestUriBuilder的底层也是使用的RestTemplate。所以也支持url级别的超时设置.只是设置稍微有点不一样:

```yaml
app:
  gateway:
    uri: localhost:8787
  http:
    hosts:
      - host: localhost:8787 #host name。域名或者ip:port  注意不需要写http://或者https://
        urls:
          - url: /user-service/empty/2/{name} #user-service是服务名.即比上面的配置多了一个服务名
            method: GET
            connectTimeout: 2000 #单位:毫秒
            readTimeout: 3000 #单位:毫秒
            writeTimeout: 4000 #单位:毫秒   
```