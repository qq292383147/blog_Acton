---
title: vue常用API
date: 2019-03-28 01:31:45
tags: vue
categories: vue
---

## 1.全局配置

> 全局配置在 <font color="#c7254e">Vue.config</font> 下，需要在启动应用之前配置

```js
import Vue from 'vue';

Vue.config.silent = false;

new Vue({
    ...
})
```

1.silent: true | false 取消Vue所有日志与警告
2.devtools: true | false 是否允许 vue-devtools 检查代码，开发版本默认为 true，生产版本默认为 false。生产版本设为 true 可以启用检查。
  keyCodes: `{f1: 112}` 给 v-on 自定义键盘事件的键名修饰符
  productionTip: true | false 显示或者阻止 vue 启动时的提示

## 2. 全局 API
### 1.Vue.extend({})

> 参数是一个组件的配置对象，结果是一个类，实例化这个类用来创建Vue组件

 

```js
 const config = {
     template: '<div>{{text}}</div>',
     data() {
        return {
           text: 'this is text'
        };
     }
  };

  // 创建构造器
  const Child = Vue.extend(config);
  const child = new Child();

  // 将实例挂在到元素上
  child.$mount('#app');
```



## 2.Vue.set(traget, key, value)

> 设置对象的属性，如果是响应式的，确保创建后也是响应式的，目标对象不能是一个 Vue 实例或 Vue 实例的根数据对象。

 

```js
 var vm=new Vue({
      el:'#app',
      data:{
          // info 属性已存在
          info:{
              name:'小明';
          }
      }
  });

  // 给info添加一个性别属性
  Vue.set(vm.info,'sex','男');
```



## 3.Vue.delete(target, key)

> 删除对象的属性，如果是响应式的，确保删除能触发更新视图

```js
  var vm=new Vue({
      el:'#app',
      data:{
          info:{
              name:'小明';
          }
      }
  });

  // 删除 info 的 name 属性
  Vue.delete(vm.info, 'name');
```



## 4.Vue.component(id, [definition])

> 注册或者获取全局组件

  

```js
// 注册组件，传入一个扩展过的构造器
  Vue.component('my-component', Vue.extend({}))

  // 注册组件，传入一个选项对象 (自动调用 Vue.extend)
  Vue.component('my-component', {tempalte: '<div></div>', ...})

  // 获取注册的组件 (始终返回构造器)
  var MyComponent = Vue.component('my-component')
```



## 5.Vue.use(plugin)

> 安装 Vue 插件, 插件需要提供 install 方法

  

```js
Vue.use(Vuex);
```



## 3.实例属性与方法

> Vue 实例暴露了一些有用的实例属性与方法，他们在创建实例时，写在配置对象中，创建实例后可以通过 
> 添加前缀 `$` 后的属性名调用，以便与用户定义的属性区分， 例如： data、data、prop。

## 1.vm.$parent

> 父实例，如果当前实例有的话。

## 2.vm.$root

> 当前组件树的根 Vue 实例。

## 3.vm.$children

> 当前实例的直接子组件

## 4.vm.slots

> 访问被插槽分发的内容 （子组件中获得插入的内容）

## 5.vm.$refs

> 一个对象，持有已注册过 ref 的所有子组件。（父组件中对子组件设置）

## 6.vm.$attrs

> 包含了父作用域中不被认为 (且不预期为) props 的特性绑定 (class 和 style 除外)。当一个组件没有声明任何 props 时，这里会包含所有父作用域的绑定 (class 和 style 除外)，并且可以通过 v-bind=”​$attrs” 传入内部组件 （子组件中调用，得到父组件传入的没有在 props 属性中声明的属性）

## 7.vm.$on(event, callback)

> 监听当前实例（调用 on()的对象）上的自定义事件。事件可以由vm.on()的对象）上的自定义事件。事件可以由vm.emit触发。回调函数会接收所有传入事件触发函数的额外参数。（是一个事件监听函数，可以监听同一个事件，做多个处理）

  

```js
vm.on('test', function (data) {})
```



## 8.vm.$emit(event, [..args])

> 触发当前实例上的事件，附加参数都会传给监听器回调

 

```js
 vm.$emit('test', 'this is data');
```

> 更加详细的使用看 组件通信部分

## 9.vm.$once(event, callback)

> 与 vm.​$on() 一致用法，监听一个自定义事件，但是只触发一次，在第一次触发之后移除监听器。

## 10.vm.$off([event, callback])

> 移除自定义事件监听器。

<font color="#c7254e">如果没有提供参数，则移除所有的事件监听器；</font><br>
<br>
<font color="#c7254e">如果只提供了事件，则移除该事件所有的监听器；</font><br>
<br>
<font color="#c7254e">如果同时提供了事件与回调，则只移除这个回调的监听器。</font><br>

## 11.vm.$mount()

> 如果 Vue 实例在实例化时没有收到 el 选项，则它处于“未挂载”状态，没有关联的 DOM 元素。可以使用 vm.​$mount() 手动地挂载一个未挂载的实例；参数为一个 dom 或者一个选择器

  

```js
const component = new MyComponent().$mount('#app');
```




## 12.vm.$forceUpdate()

> 迫使 Vue 实例重新渲染。注意它仅仅影响实例本身和插入插槽内容的子组件，而不是所有子组件。

## 13.vm.$nextTick(callback)

> 将回调延迟到下次 DOM 更新循环之后执行。在修改数据之后立即使用它，然后等待 DOM 更新。

## 14.vm.$destroy()

> 完全销毁一个实例。清理它与其它实例的连接，解绑它的全部指令及事件监听器。触发 beforeDestroy 和 destroyed 的钩子。

## 4.组件间通信

### 1.父组件向子组件传递数据

> 组件实例的作用域是孤立的，这意味着不能在子组件的模板内直接引用父组件的数据，父组件的数据需要通过 prop 才能下发到子组件中，子组件要显式的使用 props 选项声明它预期的数据

```js
  <!-- 父组件 -->
  <template>
  <div class="home-basic-container" >
      <!-- 传递 ‘prop’ 给子组件 -->
      <child :parentName="parentName"></child>
      <!-- 将所有 data 中数据传递给子组件 -->
      <child v-bind="$data"></child>
      <!-- 将一组数据传递给子组件 -->
      <child v-bind="{name: parentName, age: parentAge}"></child>
  </div>
  </template>

  <!-- 子组件 -->
  <template>
  <div class="child-container">
      <!-- 使用父组件传入的数据 -->
     {{parentName}}
  </div>
  </template>

  <script>
  export default {
      name: 'HomeBasic',
      props: ['parentName'] // 显式的声明需要的数据
  };
  </script>
```

> 注：

● prop 是单向的，父组件的变化会传给子组件，但是子组件的变化不会改变父组件的数据
● 子组件收到的 prop 只能由父组件改变，子组件内部不允许改变 prop

> 对比 : 与 react 中父组件向子组件传递数据方式类似

## 2.子组件向父组件传递信息

> 父组件可以在使用子组件的地方直接用 <font color="#c7254e">v-on</font> 来监听子组件触发的事件，在事件回调中处理来自子组件的数据
> 子组件内部可以使用 <font color="#c7254e">this.$emit(event, [...args])</font> 来触发事件

```js
<!-- 父组件 -->
<child v-bind="$data" v-on:changeName="handleNameChange"></child>
```

   

```js
 // 父组件
    data() {
        return {
            parentName: 'parent name',
            parentAge: 80
       };
    },
   methods: {
        handleNameChange(newName) {
            this.parentName = newName;
       }
    },

    <!-- 子组件 -->
    <input @input="handleChange" />

    methods: {
      handleChange(e) {
         const newName = e.target.value;
          this.$emit('changeName', newName);
      }
    },
```





> 对比：

● react 中 子组件向父组件传递数据会：在父组件中定义一个函数，用来更新状态；将这个函数像普通 prop 一样传入子组件中；子组件调用这个函数并传入参数；父组件进行相应的数据更新；
● 实测，vue 中这样的操作也可以达到目的，但是官方提供了另外一种自定义事件的方式，如上；

## 3.父组件中直接使用子组件

> 可以通过在子组件上添加 ref 属性，然后在需要使用的地方通过 ref 属性名称获取到子组件的实例

  

```html
<!-- 父组件中 -->
  <Child ref="child"></Child>
```

```js
method: {
      some() {
          this.$refs.child.doSomeThing();
      }
  }
```

> 对比：react 中有同样的方式。

## 4.子组件中直接使用父组件

> 在子组件中 直接通过 this.$parent 就可以获得父组件的实例

  

```js
method: {
      some() {
          console.log(this.$parent);
      }
  }
```

## 5.非父子组件间通信

> 1.如果两个组件有同一个父组件（不推荐的方法）

```html
<!-- 父组件 -->
  <child v-bind="$data" ref="child"></child>
  <Test v-on:changeName="handleChange"></Test>
```

```js
  // 父组件中的函数用来调用其中一个子组件的方法
  handleChange(newName) {
      this.$refs.child.changeName(newName);
  }

  // 触发事件的子组件 Test
  handleChange() {
      this.$emit('changeName', 'this is newName');
  }

  // 接收事件的子组件 Child
  changeName(newName) {
      this.showName = newName;
   }
```

> 如果两个组件间没有直接的联系（推荐）

  

```js
// 在两个组件外制作一个事件总线
  const bus = new Vue();

  // 组件 A 中触发事件
  bus.$emit('event', [...args]);

  // 组件 B 中监听事件（要在钩子函数中）
  bus.$on('event', function () {});
```



## 5.使用插槽分发内容

> 为了让组件可以组合，我们需要一种方式来混合父组件的内容与子组件自己的模板，这个过程被称为内容分发，Vue 中使用<font color="#c7254e"> `<slot>` </font>元素作为原始内容的插槽
>
>父组件模板的内容在父组件作用域内编译；子组件模板的内容在子组件的作用域内编译

## 1.单个插槽

子组件中包含一个<font color="#c7254e"> <slot></font> 插口，表示在父组件中添加的内容片段，要在子组件中显示的位置，子组件的 <font color="#c7254e"> <slot></font> 标签中可以内容，表示在父组件传入片段为空时的显示内容

 

```html
 <div>
    <h2>我是子组件的标题</h2>
    <slot>
      只有在没有要分发的内容时才会显示。
    </slot>
  </div>

  <!-- 父组件 -->

  <div>
    <h1>我是父组件的标题</h1>
    <my-component>
      <p>这是一些初始内容</p>
      <p>这是更多的初始内容</p>
    </my-component>
  </div>

  <!-- 渲染结果 -->

  <div>
    <h1>我是父组件的标题</h1>
    <div>
      <h2>我是子组件的标题</h2>
      <p>这是一些初始内容</p>
      <p>这是更多的初始内容</p>
    </div>
  </div>
```



## 2.具名插槽

> 具名插槽可以 用来分配父组件传入的内容在子组件中的位置， <font color="#c72594"><slot> </font>可以有一个 name 的特性，用来标记插槽的名字，父组件中传入的片段可以有一个 slot 的特性，值为具名插槽的名字，片段会插入到对应的具名插槽中。

```html
  <!-- 父组件 -->
  <Test v-on:changeName="handleChange">

      <p slot="a">parentName is {{parentName}}</p>
      <p>parentName is {{parentName}}</p>
      <p slot="b">parentName is {{parentName}}</p>
  </Test>

  <!-- 子组件 -->
  <slot name="a">
    名为 a 的具名插槽
  </slot>
  <button @click="handleChange">Button</button>
  <slot>
    普通插槽，父组件中没有 slot 特性的片段会显示在这里
    如果父组件中的 slot 指向的具名插槽不存在，则不显示
  </slot>
```



## 3.作用域插槽

> 作用域插槽允许在父组件中的 插入片段中使用来自子组件对应插槽的数据

 

```html
 <!-- 子组件中，需要将数据传递到插槽，就像将 prop 传递给组件一样 -->
  <slot child-info="this is child info"></slot>

  <!-- 父组件中，的子组件标签下要有含有 slot-scope 特性的 <template>标签，值为一个临时的对象，接收子组件的 prop -->
  <child>
      <template slot-scope="props">
          <p>{{props.childInfo}}</p>
      </template>
  </child>

```

> 注意：

● 作用域插槽可以是具名的也可以是非具名的
● vue 2.5 之前的版本 使用的是 scope 2.5 及其之后的版本使用的是 slot-scope
● 在 2.5.0+，slot-scope 能被用在任意元素或组件中而不再局限于