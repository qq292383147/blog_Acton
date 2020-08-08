---
title: 'VUE.js中访问地址的url带有#的问题'
date: 2019-07-10 11:24:24
tags: vue
categories: vue
---

在开发的过程中会发现，访问VUE的项目是会在访问地址后面加上#，这个#其实是VUE的HASH模式所产生的，正确点来说是因为VUE使用了HASH模式。

那么先说如果不想有#应该怎么做：修改路由Router的mode为history即可
例如在vue init webpack my-project创建项目完毕以后，在src->router->index.js里修改


```js
import Vue from 'vue'
import Router from 'vue-router'
 
Vue.use(Router)
 
export default new Router({
  mode: 'history'  //把Router的mode修改为history模式,VueRouter默认的模式为HASH模式
})

```

以上为解决办法。

那么稍微说一下HASH模式和history模式究竟是什么
首先VUE通常开发为单页面应用（SPA）：

那么既然是单页面应用，当用户要点击其他组件访问页面内的其他模块时，系统怎么样也要获取一个参数，然后根据这个参数返回不同的模块给用户使用，这时候就会有一个问题，这个传递的参数只放在后台处理的话是可以正常运作的，但如果用户使用了浏览器的 前进、后退、跳转 这三个功能呢，因为是单页面应用，访问地址是不变的，所以浏览器的 前进、后退、跳转 无法使用，那么要怎么办呢，只能是从访问地址动手脚了。

HASH模式：
HASH模式就是从访问地址动手脚，在访问地址的后面增加#并且带上需要的参数，这样后台就能对不同的参数显示不同的模块，而且 #以及后面的参数是不会被包含在HTTP请求中，因此对服务器的请求是没有影响的，更改参数也不会刷新页面。例子：网易云音乐的官网https://music.163.com/#

history模式：
history模式也是从访问地址动手脚，但是不再使用#，而是想普通的访问地址那样使用/，但如果这样请求的话，服务器是需要另外配置才行，要不然容易出现404错误，具体怎么配置请自行百度。