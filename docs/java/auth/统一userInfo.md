# 统一userInfo说明
> 聚合服务请求中台服务时，需要传递(userId、phone)参数时，应通过统一方式设置参数, 参照以下示例

## 聚合服务

### userInfo获取
controller方法添加注解，通过UserInfoHolder获取
```
    @RoleRead
    @GetMapping("/{name}")
    public ResponseObject<String> empty(@PathVariable String name){
        final String userId = UserInfoHolder.userId();
        return ResponseObject.success(userId);
    }
```

### userInfo传递
通过构造RestRequest请求中台服务时，设置请求参数（UserId、phone）传递
```
    
    RestRequest<Resp> restRequest = new RestRequest.Builder<Resp>()
                    .withBody(example)
                    .withClazz(Resp.class)
                    .withServiceName("exampleService")
                    .withUri("/example/uri")
                    .withMethod(HttpMethod.GET)
                    .withUserId("userId")
                    .withPhone("phone")
                    .build();
    Optional<ResponseObject<Resp>> optional = builder.one(restRequest);
    
```


## 中台服务
### 参数获取
controller方法添加注解，通过HeaderInfoHolder获取相关参数
```
    @HeaderRequired
    @ApiOperation(value = "获取用户实体,需提供手机号", notes = "demo")
    @GetMapping("findOne")
    public ResponseObject<String> findOneWithPhone() {
        String userId = HeaderInfoHolder.userId();
        return ResponseObject.success(userId);

    }
```