---
title: vue中computed 和 watch的异同
date: 2019-03-22 00:06:42
tags: vue
categories: vue
---

***html:***

我们要实现 第三个表单的值 是第一个和第二个的拼接，并且在前俩表单数值变化时，第三个表单数值也在变化

```js
    <div id="myDiv">
        <input type="text" v-model="firstName">
        <input type="text" v-model="lastName">
        <input type="text" v-model="fullName">
    </div>
```

js: 用watch方法来实现

```js
     new Vue({
      el: '#myDiv',
      data: {
        firstName: 'Foo',
        lastName: 'Bar',
        fullName: 'Foo Bar'
      },
      watch: {
        firstName: function (val) {
          this.fullName = val + ' ' + this.lastName
        },
        lastName: function (val) {
          this.fullName = this.firstName + ' ' + val
        }
      }
    })
```

js: 利用computed 来写

```js
      new Vue({
           el:"#myDiv",
                data:{
                    firstName:"Den",
                    lastName:"wang",
            },
            computed:{
                fullName:function(){
                    return  this.firstName  + " " +this.lastName;
                }
            }
       })
```

很容易看出 computed 在实现上边的效果时，是更简单的。

**二 、 详解 comouted 计算属性**

```
在vue的 模板内{{}}是可以写一些简单的js表达式的 ，很便利。但是如果在页面中使用大量或是复杂的表达式去处理数据，对页面的维护会有很大的影响。这个时候就需要用到computed 计算属性来处理复杂的逻辑运算。
```

1.优点： 
在数据未发生变化时，优先读取缓存。computed 计算属性只有在相关的数据发生变化时才会改变要计算的属性，当相关数据没有变化是，它会读取缓存。而不必想 motheds方法 和 watch 方法是的每次都去执行函数。

2.setter 和 getter方法：（注意在vue中书写时用set 和 get） 
setter 方法在设置值是触发。 
getter 方法在获取值时触发。

```js
    computed:{
       fullName:{
        //这里用了es6书写方法
            set(){
                 alert("set");
            },
            get(){
               alert("get");
               return  this.firstName  + " " +this.lastName;
            },
    
       }
     }
```

**三 、watch 方法**

虽然计算属性在大多数情况下是非常适合的，但是在有些情况下我们需要自定义一个watcher，在数据变化时来执行异步操作，这时watch是非常有用的。