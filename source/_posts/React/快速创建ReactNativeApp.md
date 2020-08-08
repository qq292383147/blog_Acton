---
title: 快速创建React Native App
date: 2019-12-18 11:48:55
tags: React & React Native
categories: React & React Native
---

## 第一步：安装create-react-native-app

[create-react-native-app](https://github.com/react-community/create-react-native-app)是React 社区孵化出来的一种无需构建配置就可以创建RN App的一个开源项目。

作为一个创建react native应用的脚手架工具，你可以通过如下命令完成安装：

```bash
npm install -g create-react-native-app
```

接下来就可以通过`create-react-native-app`来创建APP了：

```bash
create-react-native-app aa
cd aa
npm start
```

![1](H:\上传到git里面的md文件图片\picture\React\1.png)

> 上述命令，会为你创建一个aa的RN项目。

## create-react-native-app常用命令

```bash
  npm start
    启动本地开发服务器，这样一来你就可以通过Expo扫码将APP运行起来了。

  npm run ios
    将APP运行在iOS设备上，仅仅Mac系统支持，且需要安装Xcode。

  npm run android
    将APP运行在Android设备上，需要Android构建工具。

  npm test
    运行测试用例。
```

## 运行React Native应用

想要将上述创建的`aa`运行起来，你需要下载安装[Exponent](https://expo.io/)。

> 为了方便大家下载使用，我已将[Exponent](http://download.csdn.net/download/fengyuzhengfan/9896556)上传到网盘，供大家下载使用。

然后用Expo扫码屏幕上的二维码，aa就可以运行在Expo上了。

![2](H:\上传到git里面的md文件图片\picture\React\2.gif)

> 提示：为了确保Expo App能够正常访问到你的PC，你需要确保你的手机和PC处于同一网段内或者他们能够联通。

## 编辑App

经过上述的步骤，快速开发React Native App的环境就已经搭建好了，小伙伴门是不是迫不及待的想修改一下APP来查看运行效果了呢，接下来就可以编辑App.js来在Expo上查看运行效果哦。

## 可能存在的问题及解决方案

### ERROR: npm 5 is not supported yet

![3](H:\上传到git里面的md文件图片\picture\React\3.png)

**问题分析：** 在通过`create-react-native-app`命令创建一个React Native项目的时候，出现这个问题的原因是npm 5的一个bug所致@[npm@5 known issue tracking](https://github.com/npm/npm/issues/16991)

**解决方法**

将npm5降级到npm4，终端运行如下代码：

```bash
npm i npm@4 -g
```

然后在重新运行`create-react-native-app`即可。