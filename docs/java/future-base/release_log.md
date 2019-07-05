# 发布日志

## 发布说明

为了方便上层使用,base的发布不采用版本方式(即release).底层的版本都是SNAPSHOT.好处是不需要上层应用因为base的更新而频繁的更新.

## 日志

- 3.0-SNAPSHOT(2019-07-03)

  1. 去除mybatis-plus(有性能问题且无法解决,限制较多未来无法自由扩展),退回mybatis

  1. 集成sharding-jdbc,支持读写分离(分表分库)

  1. 集成PageHelper

  1. 提供分布式id(FutureIdGenerator)

  1. 集成druid spring boot starter

  1. PageResponseHelper与PageResponseObject名字互换

  1. 升级spring boot至2.1.6.RELEASE

  1. 升级spring cloud至Greenwich.SR2

  1. data-source更名为mysql

  1. web-mvc依赖不再默认依赖data-source.使用mysql,需要单独指定依赖`<artifactId>mysql</artifactId>`

  1. 数据库连接池改用Hikari(spring boot 2官方推荐,相较于druid而言更轻量简洁,性能高)

  1. web-mv中默认依赖`lombok`

  1. 不再默认依赖`spring-boot-starter-thymeleaf`


  TODO: 集成Mongodb

- 2.0-SNAPSHOT(2019-06-08)
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
     
