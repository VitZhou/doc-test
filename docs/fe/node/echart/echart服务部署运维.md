# 部署与启动echart服务环境

目前ECS服务器容易因为网络问题在npm安装依赖阶段出现诡异错误。
所以应该直接使用已经做好的服务镜像。

## 直接使用镜像创建

新建服务器，直接基于 `node-echart-image` 镜像创建服务。

## 流水线CI/CD

流水线更新后，会自动触发集成和部署，并重启 echart服务。

# 运维管理

## 服务开机自启动
镜像内已配置好机器重启后重新启动pm2服务相关配置。

## 手动启动服务，并自动重启echart服务

在echart服务根目录执行：

```
$ pm2 star app.js -i max --watch
```

## 停止服务
必须使用 delete命令，否则可能端口不释放。
```
$ pm2 stop app_name
$ pm2 delete app_name
```