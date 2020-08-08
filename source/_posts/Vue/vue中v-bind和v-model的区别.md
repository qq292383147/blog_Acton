---
title: vue中v-bind和v-model的区别
date: 2019-03-29 01:17:28
tags: vue
categories: vue
---

> 简单来说，区别如下：
> 1.v-bind用来绑定数据和属性以及表达式，缩写为':'
> 2.v-model使用在表单中，实现双向数据绑定的，在表单元素外使用不起作用

## 一、v-model

v-model多在表单中使用，在表单元素上创建双向绑定，根据控件类型选择正确的方法更新元素，可以绑定text、radio、checkbox、selected

### 1. 绑定text

  ```html
  <input type="text" v-model="val" />
  <p> {{val}} </p>
  ```
### 2. 绑定radio

  ```html
  <input type="radio" value="one" v-model="radioVal" />
  <input type="radio" value="two" v-model="radioVal" />
  <label for v-bind="radioval" />
  ```


  radioval的值随着选择单选框的值，会变成one 或者 two

### 3. 绑定checkBox

(1)单个勾选框，最终的值为逻辑值true和false

```html
<input type="checkbox" v-model="checkVal"/>
<label for="checkbox">{{checkVal}}</label>
```


（2）多个勾选框时，将值绑定到一个数组

```html
<input type="checkbox" value="apple" v-model="checkArray"/>
<label for="checkbox">{{apple}}</label>

<input type="checkbox" value="banana" v-model="checkArray"/>
<label for="checkbox">{{banana}}</label>

<input type="checkbox" value="pear" v-model="checkArray"/>
<label for="checkbox">{{pear}}</label>
<span>{{checkArray | json}}</span>
```


checkArray中的值会根据是否选中进行关联变化

### 4. 绑定select

  > （1）绑定到单个select
  > （2）绑定多个select时，同样适用数组

### 5. 增加参数

#### (1)lazy

将输入框的input事件改为change事件，使得输入框在change事件中更新而不是input
关于change事件和input事件的区别，简单说来是：
change事件必须是在输入框失去焦点之后才会触发，而input事件可以实时监测。

#### (2)number

将文本框输入的值都变为数字，如果是变为数字之后是NAN，则返回原始值

#### (3)debounce

>给输入框加一个很小的延迟，设置一个最小的延时，在每次敲击之后延时同步输入框的值与数据。如果每次更新都要进行高耗操作（例如在输入提示中 Ajax 请求），它较为有用。
注意 debounce 参数不会延迟 input 事件：它延迟“写入”底层数据。因此在使用 debounce 时应当用 vm.$watch() 响应数据的变化。若想延迟 DOM 事件，应当使用 debounce 过滤器。

## 二、v-bind

### 1.绑定文本


```html
<!-- 直接用v-bind或者{{}} -->
<p v-bind="message"></p>
<p>{{message}}</p>
```

### 2.绑定属性

```html
<p v-bind:src="http://...."></p>
<p v-bind:class="http://...."></p>
<p v-bind:style="http://...."></p>
```

### 3.绑定表达式

```js
{{ number + 1 }}
{{ ok ? 'YES' : 'NO' }}
{{ message.split('').reverse().join('') }}
```



### 4.绑定html

```html
<div>{{{ raw_html }}}</div>
 // 这个时候必须要使用三个{}
```

