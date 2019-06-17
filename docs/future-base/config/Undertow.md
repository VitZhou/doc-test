# undertow配置

- server.undertow.io-threads=4 # 设置IO线程数, 它主要执 行行 非阻塞的任务,它们会负责多个连接, 默认设置每个CPU核 心 一个线程
- server.undertow.worker-threads=32 # 阻塞任务线程池, 当执 行行类似servlet请求阻塞操作, undertow会从这个线程池中取得线程,它的值设置取决于系统的负载.默认值是:io-threads*8
- server.undertow.buffer-size=1024 # 每块buffer的空间大小,越小的空间被利用越充分，块多了但是会变慢
- server.undertow.buffers-per-region=1024 # 每个区分配的buffer数量量 , 所以pool的大小是buffer-size *buffers-per-region
- server.undertow.direct-buffers=true # 是否分配的直接内存