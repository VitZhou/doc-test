# Consul相关

## 1、查看consul是否运行良好:
    
```console
http://localhost:8500/v1/health/state/critical #该接口可以用来检测consul健康状态

or

consul members --http-addr localhost:8500
```
## 2、查看consul中集群状况

#### 2.1、查看集群成员

```console
curl localhost:8500/v1/status/peers
```
    
#### 2.2、查看集群leader

```console
curl localhost:8500/v1/status/leader
```

## 3、服务状态为非up状态时如何定位

首先可以先简单确认下服务状态:

```shell
curl -X GET http://ip:port/actuator/health
```

如果这一步走检测到服务状态为**down**时,可以用以下方式定位原因:

```shell
curl -X GET http://ip:port/actuator/health -H "Authorization:Basic {TOKEN}"
```

其中Token为actuator的账号密码的Base64加密(Base64(user:password)).通常dev,sit环境为:YWRtaW46YWRtaW4=

```shell
curl -X GET http://ip:port/actuator/health -H "Authorization:Basic YWRtaW46YWRtaW4="
```

## 脏数据

如果服务进程被强制kill了会导致consul中存在脏数据. 如果发现有这样的数据需要手工清理.清理方式如下:
```shell
curl -X PUT  http://localhost:8500/v1/agent/service/deregister/{serviceId}
```