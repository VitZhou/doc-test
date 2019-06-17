# Hystrix相关文档

## 1、short-circuited and no fallback available

- 原因: 由于并发太大导致的短路
- 解决方案: 调整hystrix的参数. 主要有如下:
    1. execution.timeout.enabled
    1. allowMaximumSizeToDivergeFromCoreSize
    1. maximumSize
    1. coreSize
    1. maxQueueSize
    > 具体的参数作用请查看*配置*章节
