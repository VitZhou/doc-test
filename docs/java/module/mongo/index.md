# MongoDb

## 依赖

```xml
<dependency>
    <groupId>com.chinasofti.futurelab</groupId>
    <artifactId>mongodb</artifactId>
    <version>${futurelab-base.version}</version>
</dependency>
```

## application.yml配置

```yaml
spring:
  data:
    mongodb:
      uri: mongodb://root:root@localhost:27011,localhost:27013,localhost:27012/?authSource=admin
      database: runoob
```

## MongoTemplate

```java
 @Autowired
 private MongoTemplate mongoTemplate;

 mongoTemplate.inser(demo);
```

## 需要注意的

#### 1、默认配置

MongoTemplate设置了默认的`WriteConcern`和`readPreference`

- readPreference: secondaryPreferred(即从节点优先,如果没有从节点则去主节点读取.如果设置了副本集则可以达到读写分离效果)
- WriteConcern: MAJORITY(大多数副本写入成功才算成功,提高了数据可靠性)

#### 2、事务

由于spring boot对于mongodb,mysql的事务处理都用了同一个`TransationManager`,两个数据库的事务无法同时存在,所以mongodb不能使用spring的事务托管. 如果必须使用mongodb的事务的话建议使用原生api来操作.详情请看[官网](https://docs.mongodb.com/manual/core/transactions/)

###### 2.2、多数据源(mongo,mysql)事务

如果mysql和mongo需要同时使用,且在一个事务内.建议先操作mysql,再操作mongo.这样如果mongodb报错可以使用spring的事务托管来回滚Mysql.