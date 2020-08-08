---
title: 一张图理清 Vue 3.0 的响应式系统
date: 2019-11-14 23:39:10
tags: vue
categories: vue
---

随着 Vue 3.0 Pre Alpha 版本的公布，我们得以一窥其源码的实现。Vue 最巧妙的特性之一是其响应式系统，而我们也能够在仓库的 packages/reactivity 模块下找到对应的实现。虽然源码的代码量不多，网上的分析文章也有一堆，但是要想清晰地理解响应式原理的具体实现过程，还是挺费脑筋的事情。经过一天的研究和整理，我把其响应式系统的原理总结成了一张图，而本文也将围绕这张图去讲述具体的实现过程。

![121](http://zhanglong292383147.gitee.io/picture_images/picture/vue/121.jpg)

> 文章涉及到的代码我也已经上传到仓库，结合代码阅读本文会更为流畅哦！

## **一个基本的例子**

Vue 3.0 的响应式系统是独立的模块，可以完全脱离 Vue 而使用，所以我们在 clone 了源码下来以后，可以直接在 packages/reactivity 模块下调试。

1. 在项目根目录运行 `yarn dev reactivity`，然后进入  `packages/reactivity` 目录找到产出的 `dist/reactivity.global.js` 文件。
2. 新建一个 `index.html`，写入如下代码：

```js
<script src="./dist/reactivity.global.js">
</script>
<script>
const { reactive, effect } = VueObserver
const origin = {  count: 0}const state = reactive(origin)
const fn = () => {  const count = state.count  console.log(`set count to ${count}`)}effect(fn)</script>
```

1. 在浏览器打开该文件，于控制台执行 `state.count++`，便可看到输出 `set count to 1`。

在上述的例子中，我们使用 `reactive()` 函数把 `origin` 对象转化成了 Proxy 对象 `state`；使用 `effect()` 函数把 `fn()` 作为响应式回调。当 `state.count` 发生变化时，便触发了 `fn()`。接下来我们将以这个例子结合上文的流程图，来讲解这套响应式系统是怎么运行的。

##  

## **初始化阶段**

![122](http://zhanglong292383147.gitee.io/picture_images/picture/vue/122.jpg)


在初始化阶段，主要做了两件事。

1. 把 `origin` 对象转化成响应式的 Proxy 对象 `state`。
2. 把函数 `fn()` 作为一个响应式的 effect 函数。

首先我们来分析第一件事。

大家都知道，Vue 3.0 使用了 Proxy 来代替之前的 `Object.defineProperty()`，改写了对象的 getter/setter，完成依赖收集和响应触发。但是在这一阶段中，我们暂时先不管它是如何改写对象的 getter/setter 的，这个在后续的”依赖收集阶段“会详细说明。为了简单起见，我们可以把这部分的内容浓缩成一个只有两行代码的 `reactive()` 函数：

```js
    export function reactive(target) {
        const observed = new Proxy(target, handler)  return observed
    }
```

> 完整代码在 reactive.js。这里的 `handler` 就是改造 getter/setter 的关键，我们放到后文讲解。

接下来我们分析第二件事。

当一个普通的函数 `fn()` 被 `effect()` 包裹之后，就会变成一个响应式的 effect 函数，而 `fn()` 也会被**立即执行一次**。

**由于在** **fn() 里面有引用到 Proxy 对象的属性，所以这一步会触发对象的 getter，从而启动依赖收集。**

除此之外，这个 effect 函数也会被压入一个名为”activeReactiveEffectStack“（此处为 effectStack）的栈中，供后续依赖收集的时候使用。

来看看代码（完成代码请看 effect.js）：

```js
export function effect (fn) {
    // 构造一个 effect
    const effect = function effect(...args) {
    return run(effect, fn, args)
}
// 立即执行一次
effect()  return effect
}
export function run(effect, fn, args) {
    if (effectStack.indexOf(effect) === -1) {
    try {
    // 往池子里放入当前 effect
    effectStack.push(effect)
    // 立即执行一遍
    fn()
    // fn() 执行过程会完成依赖收集，会用到 effect
    return fn(...args)
    } finally {
    // 完成依赖收集后从池子中扔掉这个 effect
    effectStack.pop()
    }
}}
```

至此，初始化阶段已经完成。接下来就是整个系统最关键的一步——依赖收集阶段。



## **依赖收集阶段**

![123](http://zhanglong292383147.gitee.io/picture_images/picture/vue/123.jpg)

这个阶段的触发时机，就是在 effect 被立即执行，其内部的 `fn()` 触发了 Proxy 对象的 getter 的时候。简单来说，只要执行到类似 `state.count` 的语句，就会触发 state 的 getter。

依赖收集阶段最重要的目的，就是建立一份”依赖收集表“，也就是图示的”targetMap"。targetMap 是一个 WeakMap，其 key 值是~~当前的 Proxy 对象 `state`~~代理前的对象`origin`，而 value 则是该对象所对应的 depsMap。

depsMap 是一个 Map，key 值为触发 getter 时的属性值（此处为 `count`），而 value 则是**触发过该属性值**所对应的各个 effect。

还是有点绕？那么我们再举个例子。假设有个 Proxy 对象和 effect 如下：

```js
const state = reactive({  count: 0,  age: 18})
const effect1 = effect(() => {  console.log('effect1: ' + state.count)})
const effect2 = effect(() => {  console.log('effect2: ' + state.age)})
const effect3 = effect(() => {  console.log('effect3: ' + state.count, state.age)})
```

那么这里的 targetMap 应该为这个样子：

![124](http://zhanglong292383147.gitee.io/picture_images/picture/vue/124.jpg)

这样，`{ target -> key -> dep }` 的对应关系就建立起来了，依赖收集也就完成了。代码如下：



```js
export function track (target, operationType, key) {
    const effect = effectStack[effectStack.length - 1]
    if (effect) {
    let depsMap = targetMap.get(target)
    if (depsMap === void 0) {
    targetMap.set(target, (depsMap = new Map()))
    }
    let dep = depsMap.get(key)
    if (dep === void 0) {
    	depsMap.set(key, (dep = new Set()))
    }
    if (!dep.has(effect)) {
    	dep.add(effect)
    }
}}
```

弄明白依赖收集表 targetMap 是非常重要的，因为这是整个响应式系统核心中的核心。

##  

## **响应阶段**

回顾上一章节的例子，我们得到了一个 `{ count: 0, age: 18 }` 的 Proxy，并构造了三个 effect。在控制台上看看效果：

![125](http://zhanglong292383147.gitee.io/picture_images/picture/vue/125.jpg)

效果符合预期，那么它是怎么实现的呢？首先来看看这个阶段的原理图：

![126](http://zhanglong292383147.gitee.io/picture_images/picture/vue/126.jpg)

当修改对象的某个属性值的时候，会触发对应的 setter。

setter 里面的 trigger() 函数会从依赖收集表里找到当前属性对应的各个 dep，然后把它们推入到 `effects` 和 `computedEffects（计算属性）` 队列中，最后通过 `scheduleRun()` 挨个执行里面的 effect。

由于已经建立了依赖收集表，所以要找到属性所对应的 dep 也就轻而易举了，可以看看具体的代码实现：



```js
export function trigger (target, operationType, key) {
    // 取得对应的 depsMap
    const depsMap = targetMap.get(target)
    if (depsMap === void 0) {
    	return
    }
    // 取得对应的各个 dep
    const effects = new Set()
    if (key !== void 0) {
        const dep = depsMap.get(key)
        dep && dep.forEach(effect => {
        effects.add(effect)
        })
    }
    // 简化版 scheduleRun，挨个执行 effect
    effects.forEach(effect => {
        effect()
    })
}
```

> 这里的代码没有处理诸如数组的 length 被修改的一些特殊情况，感兴趣的读者可以查看 vue-next 对应的源码，或者这篇文章，看看这些情况都是怎么处理的。

至此，响应式阶段完成。

##  

## **总结**

阅读源码的过程充满了挑战性，但同时也常常被 Vue 的一些实现思路给惊艳到，收获良多。本文按照响应式系统的运行过程，划分了”初始化“，”依赖收集“和”响应式“三个阶段，分别阐述了各个阶段所做的事情，应该能够较好地帮助读者理解其核心思路。最后附上文章实例代码的仓库地址，有兴趣的读者可以自行把玩：

tiny-reactive（https://github.com/jrainlau/tiny-reactive）