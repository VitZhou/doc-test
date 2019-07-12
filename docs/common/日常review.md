# 日常Review

每周二由架构组对前端,聚合,中台服务的前一周的代码进行一天的review.

## review方式

在需要修改的地方写上注释://@reject: 修改建议. 例如:

```java
RestRequest<String> restRequest2 = new RestRequest.Builder<String>() //@reject: 变量名需要直观有意义
                .withClazz(String.class)
                .withServiceName("demo-middle")
                .withUri("empty/" + id)
                .withMethod(HttpMethod.GET)
                .build();
``` 

业务开发人员修改完成之后将@reject改成@fixed.如:
```java
RestRequest<String> restRequest2 = new RestRequest.Builder<String>() //@fixed: 变量名需要直观有意义
                .withClazz(String.class)
                .withServiceName("demo-middle")
                .withUri("empty/" + id)
                .withMethod(HttpMethod.GET)
                .build();
```

架构组人员验收觉得可以之后可以删除@fixed注释. 如果觉得还需要调整则继续改回@reject并写上相关建议.

## review范围

对新功能的代码进行review,旧代码留下一个季度时间修改. 11月1日起开始review旧代码

## 要求

1. 所有的打了@reject的,需要重构的地方原则都需要当周解决.
2. 在每周五下午进行的`代码质量竞赛`上,如果看到了有未重构的@reject存在,发现一次扣1分.
3. 如果自己删除了@reject的,发现一次扣10分

## 职责

- 架构组人员: 负责review,验收以及指导开发人员进行代码重构
- 各组leader: 负责监督各组成员完成重构

## idea提示

如图配置即可让idea提示架构人员标记的需要修改的地方:

![](http://ww1.sinaimg.cn/large/006tNc79gy1g4x0is5nvgj31dk0u0ha5.jpg)

然后再idea中点击TODO功能即可看到提示:

![](http://ww4.sinaimg.cn/large/006tNc79gy1g4x0lkxst9j31qp0u0dtv.jpg)