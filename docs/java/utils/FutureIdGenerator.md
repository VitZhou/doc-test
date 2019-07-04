# 分布式id生成器

## 使用方式

```java
public static void main(String[] args) {
    long id = FutureIdGenerator.getId();
    String timeId = FutureIdGenerator.getTimeId();
    String idStr = FutureIdGenerator.getIdStr();
    String millisecond = FutureIdGenerator.getMillisecond();
    String uuid = FutureIdGenerator.get32UUID();
    System.out.println("id:"+id);
    System.out.println("timeId:"+timeId);
    System.out.println("idStr:"+idStr);
    System.out.println("millisecond:"+millisecond);
    System.out.println("uuid:"+uuid);
}
```

输出如下:

```console
id:1146763521904640001
timeId:201907042051441281146763521963360258
idStr:1146763521963360259
millisecond:20190704205144130
uuid:b3e148b2ed2e80abb482d01959ff1dbc
```

> timeId建议在`订单号`之类的场景中

