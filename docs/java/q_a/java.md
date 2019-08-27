# Java相关问题

## java进程自动重启

自动重启一般是不会发生的,但是如果系统的内存不足的会会导致系统进程杀死java内存.一般情况下内存溢出是会dump出来的,目录在`/data/logs`。
但是一旦发生系统自动重启的情况下,会没有dump文件,因为java进程被系统进程杀死了. 这种情况需要去系统日志中查看什么原因.系统日志目录为:`/var/log`,在该目录下的message文件会记录系统日志.通过以下方式可以定位到进程被杀的原因

```shelll
cat messages | grep java
cat messages | grep Kill
```

你会看到类似如下内容:

```log
Jun 29 20:11:39 uat-base-user-service-a kernel: Out of memory: Kill process 10043 (java) score 714 or sacrifice child
Jun 29 20:11:39 uat-base-user-service-a kernel: Killed process 10043 (java) total-vm:3118132kB, anon-rss:745304kB, file-rss:0kB, shmem-rss:0kB
```

## 找出CPU占用最高的线程
有时候cpu持续彪高,此时需要找到彪高的原因.步骤如下

1. 通过top命令(shift+p 按cpu排序，shift+m 按内存排序)找出消耗cpu最多的进程
1. 使用top -Hp pid获取进程下面所有的线程.
   ![](https://raw.githubusercontent.com/VitZhou/pic/master/20190827164349.png)
   > 注意这里的PID是线程id
1. printf '%x\n' PID(上一步骤获取到的PID)获取到线程号的16进制.假设为5c7d
1. 使用jstack获取对应线程信息
jstack {进程id} | grep {16进制线程id号}。即可看到占用cpu最多的进程
![](https://raw.githubusercontent.com/VitZhou/pic/master/20190827164747.png)
1. 最后用jstack pid>xxx.dump导出堆栈.根据上图的线程即可判断是什么原因导致的cpu过高
