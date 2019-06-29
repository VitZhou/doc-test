# Actuator说明

说明: 大部分actuator信息都需要权限才能查看。其中Token为actuator的账号密码的Base64加密(Base64(user:password)).通常dev,sit环境为:YWRtaW46YWRtaW4=,一般请求格式如下:

```shell
curl -X GET http://ip:port/actuator/health -H "Authorization:Basic YWRtaW46YWRtaW4="
```

- actuator/health: 健康状态,如果带上权限信息可以看到详细的监控状态.
- actuator/configprops: 查看所有已生效配置
- actuator/prometheus.plan: 查看prometheus相关的监控信息.
- actuator/beans: 所有的spring管理的bean
- actuator/info: 服务信息,包括构建信息,git提交信息,可以用于确定生产上发布的版本是否正确.
- actuator/metrics: 该端点用来返回当前应用的各类重要度量指标，比如：内存信息、线程信息、垃圾回收信息等
- actuator/trace: 该端点用来返回基本的HTTP跟踪信息，默认情况下，跟踪信息的存储采用org.springframework.boot.actuate.trace.InMemoryTraceRepository实现的内存方式，始终保留最近的100条请求记录
- actuator/dump: 类似jstack
- actuator/shutdown: 优雅关闭
