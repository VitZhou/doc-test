# PageHelper

我们使用了开源的PageHelper作为分页工具.

> 它有一定的局限性,使用之前一定先看下[这里](https://github.com/pagehelper/Mybatis-PageHelper/blob/master/wikis/zh/Important.md)

## 配置

pageHelper需要配置如下内容:

```yaml
pagehelper:
  helperDialect: mysql
  reasonable: true
  supportMethodsArguments: true
  params: count=countSql
```

> 这些配置已经在配置中心的顶层仓库中配置,业务开发中不需要再配置

## 如何使用

只要你的依赖中有mysql的依赖即可使用其api. 它的api方式有非常多种。这里推荐使用以下两种方式:

1. Page

   ```java
   public PageResponseObject<UserEntity> findUser(int startPage, int pageSize) {
       Page<UserEntity> page = PageHelper.startPage(startPage, pageSize).doSelectPage(() -> userResource.find());
       return PageResponseHelper.success(page);
   }
   ```

2. PageInfo

   ```java
   public PageResponseObject<UserEntity> findUser(int startPage, int pageSize) {
       PageInfo<UserEntity> page =
           PageHelper.startPage(startPage, pageSize).doSelectPageInfo(() -> userResource.find());
       return PageResponseHelper.success(page);
   }
   ```

`注意`:这种使用方式在mybatis的xml文件不需要写`limit`,pageHelper会自动补全:

```xml
<select id="findList" resultMap="BaseResultMap">
    select * from t_user
</select>
```

如果你需要其他的使用方式可以查看[官网](<https://github.com/pagehelper/Mybatis-PageHelper/blob/master/README_zh.md>)

