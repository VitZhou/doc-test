# Feign配置

## 超时

支持url级别的超时设置. 例如:

```yaml

app:
  http:
    services:
      - serviceName: userService #服务名
        urls:
          - url: /empty/2/{name} #url
            method: GET
            connectTimeout: 2000 #单位:毫秒
            readTimeout: 3000 #单位:毫秒
```