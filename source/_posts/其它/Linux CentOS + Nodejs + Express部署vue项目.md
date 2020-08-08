---
title: Linux CentOS + Nodejs + Express部署vue项目
date: 2019-12-21 23:01:44
type: 其它
tags: 其它
categories: 其它
---

注：服务器为CentOS 7.3.1611，使用Xshell6 + Xftp6工具完成服务器远程操作

## 一、安装Node环境

通过Xshell连接服务器成功之后就可以开始以下工作

### 1.清理工作

如果之前有安装过nodejs，用自带的包管理命名先删除一次

```bash
yum remove nodejs npm -y
```



然后手动进入以下目录删除相关文件
进入 /usr/local/lib 删除所有 node 和 node_modules文件夹
进入 /usr/local/include 删除所有 node 和 node_modules 文件夹
进入 /usr/local/bin 删除 node 的可执行文件

### 2.去官网复制node安装包链接

https://nodejs.org/en/download/
![24](H:\上传到git里面的md文件图片\picture\other\24.png)

### 3.在Xshell里cd到安装目录

```bash
cd /usr/local/
```

### 4.输入命令链接开始下载nodejs安装包

```bash
wget https://nodejs.org/dist/v10.16.0/node-v10.16.0-linux-x64.tar.xz
```

### 5.输入命令两步解压

```bash
xz -d node-v10.16.0-linux-x64.tar.xz
tar -xvf node-v10.16.0-linux-x64.tar
```

### 6.重名解压的文件夹名称为nodejs

```bash
mv node-v10.16.0-linux-x64 nodejs
```

### 7.进入解压目录

```bash
cd nodejs
```

### 8.创建软连接

```bash
ln -s  /usr/local/nodejs/bin/node /usr/local/bin/node
ln -s  /usr/local/nodejs/bin/npm /usr/local/bin/npm
```

如果不小心输错了路径，重新创建会提示：‘ln: 无法创建符号链接"/usr/local/bin/npm": 文件已存在’，输入rm /usr/local/bin/npm命令清除后可以重新创建

### 9.测试

```bash
node -v
npm -v
```

### 10.安装cnpm淘宝镜像并创建软链接

```bash
npm install -g cnpm
ln -s /usr/local/nodejs/bin/cnpm /usr/local/bin/cnpm
```



## 二、用Express搭建web服务

### 1.在Xshell里cd到指定目录

```bash
cd /var/www/
```



注：如果没有www目录就在var目录下输入命令mkdir www手动创建一个，并进入到www目录

### 2.创建web服务项目文件夹

```bash
mkdir demo
```

### 3.cd进入项目目录

```bash
cd demo
```

### 4.初始化项目生成package.json

```bash
npm init -y
```

注：这里的-y意思是省略创建过程中一直输yes的步骤

### 5.安装express

```bash
cnpm i express -D
```

### 6.创建web服务程序文件app.js

```bash
mkdir app.js
```

### 7.编写web服务程序代码app.js

```js
const fs = require('fs'); //文件模块
const path = require('path'); //路径模块
const express = require('express'); //express框架模块
const app = express();
const hostName = '11.22.33.44'; //ip
const port = 9999; //端口

app.use(express.static(path.resolve(__dirname, './dist'))); // 设置静态项目访问路径(此处的dist为webpack打包生成的项目文件夹名称)

app.get('*', function(req, res) {
    const html = fs.readFileSync(path.resolve(__dirname, './dist/index.html'), 'utf-8'); // 设置所有访问服务请求默认返回index.html文件
    res.send(html);
});

app.listen(port, hostName, function() {
    console.log(服务器运行在http://${hostName}:${port});
});
```


## 三、打包部署vue项目

### 1.在本地开发工具里打包需要部署的vue项目

```bash
npm run build
```

生成的dist文件夹就是我们需要部署到服务器上的项目

![25](H:\上传到git里面的md文件图片\picture\other\25.png)

## 2.把dist文件夹通过Xftp工具复制到服务器的var/www/demo目录下

![26](H:\上传到git里面的md文件图片\picture\other\26.png)

## 四、启动web服务



### 1.在Xshell里cd到var/www/demo目录，输入以下命令启动web服务程序

```bash
node app.js
```



如果能正常访问项目地址表示已经搭建成功。

请求后端接口跨域方案请见：
跨域代理方案1：Nginx使用教程
跨域代理方案2：Nodejs 中使用http-proxy-middleware实现代理跨域

### 2.安装PM2托管Node Web服务程序

在xshell里用node默认的启动方式有一个缺点，xshell退出后nodejs项目便会停止
使用pm2这个托管工具可以很好的解决这个问题，而且当代码有更改时会自动重启服务更新

### 1.首先多按两次ctrl +c结束之前的运行程序，接着输入下面的命令安装pm2并创建软链接

```bash
cnpm install pm2 -g
ln -s /usr/local/nodejs/bin/pm2 /usr/local/bin/pm2
```



### 2.然后输入下面的命令启动托管任务，abc为托管项目定义的名称

```bash
pm2 start app.js --name abc
```



### 以下为pm2常用命令说明

| 命令                        | 功能                                              |
| --------------------------- | ------------------------------------------------- |
| pm2 start app.js --name abc | 启动(--name为定义任务名称的指令，abc为任务名称值) |
| pm2 start app.js --watch    | 启动( --watch为监听应用目录的变化的指令)          |
| pm2 restart app.js          | 重启任务                                          |
| pm2 stop abc                | 结束(abc为任务名称或id)                           |
| pm2 list                    | 查看所有任务列表                                  |

### pm2基本功能命令

| 功能                  | 命令                                  |
| --------------------- | ------------------------------------- |
| 启动进程/应用         | pm2 start bin/abc 或 pm2 start app.js |
| 重命名进程/应用       | pm2 start app.js --name abc           |
| 添加进程/应用         | pm2 start bin/abc --watch             |
| 结束进程/应用         | pm2 stop abc                          |
| 结束所有进程/应用     | pm2 stop all                          |
| 删除进程/应用         | pm2 delete abc                        |
| 删除所有进程/应用     | pm2 delete all                        |
| 列出所有进程/应用     | pm2 list                              |
| 查看进程/应用详情     | pm2 show abc 或 pm2 describe abc      |
| 查看进程/应用资源消耗 | pm2 monit                             |
| 查看进程/应用日志     | pm2 logs abc                          |
| 查看所有进程/应用日志 | pm2 logs                              |
| 重新启动进程/应用     | pm2 restart abc                       |
| 重新启动所有进程/应用 | pm2 restart all                       |

pm2使用教程参考链接：
https://www.cnblogs.com/chyingp/p/pm2-documentation.html
https://www.jb51.net/article/113398.htm
