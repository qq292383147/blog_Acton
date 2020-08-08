---
title: 如何远程部署CentOS前后端项目的思路
date: 2019-12-21 22:51:50
type: 其它
tags: 其它
categories: 其它
---

## 学习到的相关知识

-  远程登录 CentOS 系统
-  安装node、npm、nginx、pm2、xftp、
-  使用linux常用命令
-  nginx 配置
-  node + express 开发
-  绑定域名、域名备案

## 远程登录 CentOS 系统

在本机电脑打开窗口命名,输入以下命令,

```bash
ssh root@公网ip 
```

回车后，输入实例密码，出现`Welcome to Alibaba Cloud Elastic Compute Service !` 表示登录成功。

登录成功后,初始化是没有任何图形化界面的，刚刚开始我练习了一些`linux`命令，如

- 查看目录 ls
- 新建文件夹 mkdir

更多命令在 [菜鸟教程](https://www.runoob.com/linux/linux-tutorial.html) 中可以查询。

## 安装node环境

熟悉了一些命令后，准备完成我的任务，搭建`node`环境

- 安装node命令

```bash
wget https://nodejs.org/dist/v9.3.0/node-v9.3.0-linux-x64.tar.xz
```

安装后输入ls发现这是个压缩包，还需要用命令进行解压

- tar.gz.zip解压命令

```bash
tar负责打包，gzip负责压缩
tar
-c: 建立压缩档案
-x：解压
-t：查看内容
-r：向压缩归档文件末尾追加文件
-u：更新原压缩包中的文件
```

- 解压安装包后配置node环境变量

```bash
ln -s ~/node-v9.3.0-linux-x64/bin/node /usr/bin/node
ln -s ~/node-v9.3.0-linux-x64/bin/npm /usr/bin/npm
```

## nginx配置

安装nginx后， 输入  `vim nginx.conf` 命令 打开 nginx.conf 文件，进行配置, 我的文件大概是下面的样子

```bash
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/

# user nginx;
user root;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /var/run/nginx.pid;

# Load dynamic modules. See /usr/share/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
   worker_connections  1024;
}
http {
   log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                     '$status $body_bytes_sent "$http_referer" '
                     '"$http_user_agent" "$http_x_forwarded_for"';

   access_log  /var/log/nginx/access.log  main;

   sendfile            on;
   tcp_nopush          on;
   tcp_nodelay         on;
   keepalive_timeout   65;
   types_hash_max_size 2048;

   include             /etc/nginx/mime.types;
   default_type        application/octet-stream;

   # Load modular configuration files from the /etc/nginx/conf.d directory.
   # See http://nginx.org/en/docs/ngx_core_module.html#include
   # for more information.
   include /etc/nginx/conf.d/*.conf;
   server {
   # 访问端口号
   listen       80;
   # 服务器地址(访问ip)
   server_name  公网ip;
 
   include /etc/nginx/default.d/*.conf;
   # 静态资源文件
   location / { 
     # 静态资源存放路径
     root /root/nodeChatRoom/client/; 
     # 默认读取文件
     index index.html index.htm;     
   }
   # 接口代理
   location /api {
     # 后端代码接口地址，node启动地址
     proxy_pass http://公网ip:3000;
   }
   error_page 404 /404.html;
       location = /40x.html {
   } 
   }
}
```

配置好nginx 后，重启nginx

```bash
 nginx -s reload
```

## 安装xftp

开发环境配置好后,如何把我本机的代码上传到远程服务呢, 我选择的是xftp,这样修改文件就不需要使用`linux`命令了,也方便上传文件。

## pm2

- 安装pm2

```bash
sudo npm uninstall pm2
sudo npm i pm2@latest -g
sudo ln -s  /root/node-v9.3.0-linux-x64/lib/node_modules/pm2/bin/pm2 /usr/local/bin/pm2
```

把node项目上传到远程电脑后, 打开命令行，输入

```bash
node app.js
```

后台服务启动成功, 但是这样把命令窗口关闭,服务就停止了,如果我重新上传了新的node代码,服务不会更新, 所以我选择了`pm2`,启动`node`服务, 自动更新和启动我的后台服务。

## 技巧

- 安装tree 查看tree目录

```js
yum install tree
tree // 在node文件夹下查看树形目录
```

出现大概下面的目录结构

```
.
├── bin
│   ├── node
│   ├── npm -> ../lib/node_modules/npm/bin/npm-cli.js
│   └── npx -> ../lib/node_modules/npm/bin/npx-cli.js
├── CHANGELOG.md
├── include
```

- 通过 whereis 查看软件所在路径 如查看 node 所在路径

```bash
whereis node
```

