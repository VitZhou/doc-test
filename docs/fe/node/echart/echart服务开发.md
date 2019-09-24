# 安装
## 安装系统依赖

OS | Command
----- | -----
OS X | `brew install pkg-config cairo pango libpng jpeg giflib`
Ubuntu | `sudo apt-get install libcairo2-dev libjpeg8-dev libpango1.0-dev libgif-dev build-essential g++`
Fedora | `sudo yum install cairo cairo-devel cairomm-devel libjpeg-turbo-devel pango pango-devel pangomm pangomm-devel giflib-devel`
Solaris | `pkgin install cairo pango pkg-config xproto renderproto kbproto xextproto`
Windows | [Instructions on our wiki](https://github.com/Automattic/node-canvas/wiki/Installation---Windows)

## 安装node依赖

`npm install`

## 运行服务器

`node app.js`

# 开发 

```xml
nodechart
 |-demo/ demo代码，可以把修改后的代码复制进去修改运行看效果
 |-app.js //node 服务入口，包含路由
 |-index.js // 封装的echart方法
 |-colorbar1.js // canvas生成coloarbar1类型图片
 |-colorbar2.js // canvas生成coloarbar2类型图片
 |-colorbar3.js // canvas生成coloarbar3类型图片
 |-colorTbar.js // canvas生成coloarbar4类型图片
```