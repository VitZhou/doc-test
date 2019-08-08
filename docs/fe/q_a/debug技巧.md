# Debug 技巧

## 手机

推荐利用 `vConsole`，获取调试信息。

**需要调试问题时**

在html内插入如下代码

```html
<script src="path/to/vconsole.min.js"></script>
<script>
  // init vConsole
  var vConsole = new VConsole();
  console.log('Hello world');
</script>
```

之后讲在同一局域网网段下用手机访问页面。

提交代码前，务必记得清掉vConsole调用代码。

最新的 vConsole可以在这里获取：
https://www.bootcdn.cn/vConsole/

## PC

打开浏览器控制台。
关注error类型的log。

常见问题分析：

对象操作相关：
```
Uncaught TypeError: Cannot Read Property
TypeError: ‘undefined’ Is Not an Object (evaluating...)
TypeError: Null Is Not an Object (evaluating...)
```
十有八九都是没做非空判断，对一个不存在或者为空的对象进行了 `.` 运算符操作。
