# ZipKin配置

- spring.sleuth.sampler.probability: 采样率(0-1.0),默认为0.1(即10个请求里面采样一次)
- spring.sleuth.sampler.rate: 采样速率,限制每秒钟的采样次数.默认不限制,最小值0,最大值2,147,483,647(int的最大值)