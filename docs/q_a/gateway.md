# gateway相关问题

## actuator相关接口

所有的接口的请求方式均为: ip:port/uri 开发测试环境的账号密码均为admin:admin
- GET /actuator/gateway/globalfilters: 全局过滤器
- GET /actuator/gateway/routes: 所有的路由规则
- GET /actuator/gateway/routefilters: 路由过滤器
- POST /actuator/gateway/refresh: 刷新路由缓存(如果有新服务上线,且gateway还没发现该服务,可以尝试用该接口手工刷新)
- GET /actuator/gateway/routes/{id}: 检索特定的路由.注意id是路由id并不是服务名
- POST /gateway/routes/{id_route_to_create}: 添加新的路由
- DELETE /gateway/routes/{id_route_to_delete}: 删除指定路由


## 请求都需要带上账号密码才能有响应

```shell
curl -X POST  http://ip:port/actuator/gateway/routes -H "Authorization:Basic {Base64 user:password}"
```


## 503 Service Unavailable

consul机制,将非**up**健康状态的服务视为不可用,所以如果gateway返回这种状态时可以用consul的health接口检测服务状态.

```yaml
http://localhost:8500/v1/health/service/{serviceName}
```

## 404问题

如果服务刚上线可能会存在404问题,所以你需要先在consul中查看你的服务是否已经上线且健康状态为**up**状态. 如果确认没有问题,则需要等待一个consul的刷新间隔(默认是30秒,我们已经将其改为10秒).
如果过了间隔时间仍然没有则可以尝试调用refresh接口清空gateway的缓存强制刷新.