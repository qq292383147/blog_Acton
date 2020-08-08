---
title: vue store存储commit和dispatch
date: 2019-10-22 22:14:05
tags: vue
categories: vue
---


# vue store存储commit和dispatch




```
this.$store.commit('toShowLoginDialog', true);
this.$store.dispatch('toShowLoginDialog',false)
主要区别是：

dispatch：含有异步操作，例如向后台提交数据，

写法： this.$store.dispatch('mutations方法名',值)
commit：同步操作，写法：this.$store.commit('mutations方法名',值)

例如：登录成功后读取用户信息写到coikie里
```

===============
Vue的项目中，如果项目简单， 父子组件之间的数据传递可以使用  props 或者 $emit 等方式 进行传递

但是如果是大中型项目中，很多时候都需要在不相关的平行组件之间传递数据，并且很多数据需要多个组件循环使用。
这时候再使用上面的方法会让项目代码变得冗长，并且不利于组件的复用，提高了耦合度。

Vue 的状态管理工具 Vuex 完美的解决了这个问题。

```shell
npm install vuex --save
```

然后在src目录下新建文件夹store，在文件夹内新建store.js文件，这个文件我们用来组装模块并导出 store 的文件
在main.js文件中注册store

```js
import store from './store/store'

new Vue({
el: '#app',
router,
store,
template: '<App/>',
components: { App }
})
```

