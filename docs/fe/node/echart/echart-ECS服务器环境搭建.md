如果服务器和镜像备份均被删除，切无任何备份，
需要重新安装各种依赖。

### 系统底层依赖
OS | Command
----- | -----
OS X | `brew install pkg-config cairo pango libpng jpeg giflib`
Ubuntu | `sudo apt-get install libcairo2-dev libjpeg8-dev libpango1.0-dev libgif-dev build-essential g++`
Fedora | `sudo yum install cairo cairo-devel cairomm-devel libjpeg-turbo-devel pango pango-devel pangomm pangomm-devel giflib-devel`
Solaris | `pkgin install cairo pango pkg-config xproto renderproto kbproto xextproto`
Windows | [Instructions on our wiki](https://github.com/Automattic/node-canvas/wiki/Installation---Windows)

### 中文字体

部分Linux没有中文字体。
如果缺少字体会导致部分echart图表中文乱码。
centos:

`yum -y groupinstall Fonts`

### 安装nodejs

使用NVM版本管理器安装多版本。
登录弹性云服务器。

1. 执行以下命令，安装git。
`yum install git`

2. 执行以下命令，使用git将源码克隆到本地的~/.nvm目录下，并检查最新版本。
`git clone https://github.com/cnpm/nvm.git ~/.nvm `
`cd ~/.nvm`
``git checkout `git describe --abbrev=0 --tags` ``

1. 执行以下命令，激活NVM，并将其追加至profile文件下。
`echo ". ~/.nvm/nvm.sh" >> /etc/profile`

4. 执行如下命令，使环境变量生效。
`source /etc/profile`

5. 执行以下命令，列出Node.js可用版本。
`nvm ls-remote`

6. 执行以下命令，安装指定Node.js版本。
`nvm install v8.12.0`

7. 执行以下命令，查看已安装的Node.js版本。
`nvm ls`

8. 执行以下命令切换Node.js版本至v8.12.0。
`nvm use v8.12.0`

9. 执行以下命令设置默认版本
`nvm alias default v8.12.0`

10. 测试node是否运作正常
    执行 `node -v`

### 安装pm2和cnpm

全局安装
`npm install cnpm pm2  -g  --registry=https://registry.npm.taobao.org`

## 安装node服务依赖
建议使用cnpm，否则可能有一些依赖因为国内网络环境问题无法安装上。

在echart服务根目录执行：
`cnpm install`
