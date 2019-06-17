# 发布日志

## 发布说明

为了方便上层使用,base的发布不采用版本方式(即release).底层的版本都是SNAPSHOT.好处是不需要上层应用因为base的更新而频繁的更新.

## 日志

- 2019-06-08
  1. 服务发现
  2. 配置中心
  3. prometheus的jmx和服务监控
  4. zipkin链路监控
  5. redis的简单封装
  6. webmvc
     1. 404,500,401状态码的统一处理
     2. 异常统一处理(注意异常的处理最好不要依赖底层框架,底层框架只是用来兜底的.业务能处理的话最好自己处理)
        - AppBusinessException
        - BaseException
        - SQLDataException
        - IllegalArgumentException
        - ServiceUnavailableException
     3. 请求日志的记录
     4. restTemplate和feign底层通信使用了OkHttp
     5. 基础配置(具体请看配置相关文档)
  7. zookeeper
     1. 基于zk的分布式调度(具体请查看分布式调度文档)
