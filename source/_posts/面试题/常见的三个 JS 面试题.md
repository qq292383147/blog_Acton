---
title: 常见的三个 JS 面试题
date: 2019-10-22 22:04:05
tags: 面试题
type: 面试题
categories: 面试题
---



### 问题 1: 事件委托代理

在构建应用程序时，有时需要将事件绑定到页面上的按钮、文本或图像，以便在用户与元素交互时执行某些操作。

如果我们以一个简单的待办事项列表为例，面试官可能会告诉你，当用户点击列表中的一个列表项时执行某些操作。他们希望你用 JavaScript 实现这个功能，假设有如下 HTML 代码:

```html
<ul id="todo-app">
  <li class="item">Walk the dog</li>
  <li class="item">Pay bills</li>
  <li class="item">Make dinner</li>
  <li class="item">Code for one hour</li>
</ul>
```

你可能想要做如下操作来将事件绑定到元素:

```js
document.addEventListener('DOMContentLoaded', function() {
  let app = document.getElementById('todo-app');
  let itimes = app.getElementsByClassName('item');

  for (let item of items) {
    item.addEventListener('click', function(){
      alert('you clicked on item: ' + item.innerHTML);
    })
  }
})
```

虽然这在技术上是可行的，但问题是要将事件分别绑定到每个项。这对于目前 `4` 个元素来说，没什么大问题，但是如果在待办事项列表中添加了 `10,000` 项(他们可能有很多事情要做)怎么办?然后，函数将创建 10,000 个独立的事件侦听器，并将每个事件监听器绑定到 DOM ，这样代码执行的效率非常低下。

在面试中，最好先问面试官用户可以输入的最大元素数量是多少。例如，如果它不超过 `10`，那么上面的代码就可以很好地工作。但是如果用户可以输入的条目数量没有限制，那么你应该使用一个更高效的解决方案。

如果你的应用程序最终可能有数百个事件侦听器，那么更有效的解决方案是将一个事件侦听器实际绑定到整个容器，然后在单击它时能够访问每个列表项， 这称为 **事件委托**，它比附加单独的事件处理程序更有效。

下面是事件委托的代码:

```js
document.addEventListener('DOMContentLoaded', function() {
  let app = document.getElementById('todo-app');

  app.addEventListener('click', function(e) {
    if (e.target && e.target.nodeName === 'LI') {
      let item = e.target;
      alert('you clicked on item: ' + item.innerHTML)
    }
  })
})
```



## 问题 2： 在循环中使用闭包

闭包常常出现在面试中，以便面试官衡量你对 JS 的熟悉程度，以及你是否知道何时使用闭包。

闭包基本上是内部函数可以访问其范围之外的变量。 闭包可用于实现隐私和创建函数工厂， 闭包常见的面试题如下：

> 编写一个函数，该函数将遍历整数列表，并在延迟3秒后打印每个元素的索引。

经常不正确的写法是这样的：

```js
const arr = [10, 12, 15, 21];
for (var i = 0; i < arr.length; i++) {
  setTimeout(function() {
    console.log('The index of this number is: ' + i);
  }, 3000);
}
```

如果运行上面代码，`3` 秒延迟后你会看到，实际上每次打印输出是 `4`，而不是期望的 `0，1，2，3` 。

为了正确理解为什么会发生这种情况，了解为什么会在 JavaScript 中发生这种情况将非常有用，这正是面试官试图测试的内容。

原因是因为 `setTimeout` 函数创建了一个可以访问其外部作用域的函数（闭包），该作用域是包含索引 `i` 的循环。 经过 `3` 秒后，执行该函数并打印出 `i` 的值，该值在循环结束时为 `4`，因为它循环经过`0,1,2,3,4`并且循环最终停止在 `4`。

实际上有[多处](https://stackoverflow.com/questions/3572480/please-explain-the-use-of-javascript-closures-in-loops)方法来正确的解这道题：

```js
const arr = [10, 12, 15, 21];

for (var i = 0; i < arr.length; i++) {
  setTimeout(function(i_local){
    return function () {
      console.log('The index of this number is: ' + i_local);
    }
  }(i), 3000)
}
```

```js
const arr = [10, 12, 15, 21];
for (let i = 0; i < arr.length; i++) {
  setTimeout(function() {
    console.log('The index of this number is: ' + i);
  }, 3000);
}
```



## 问题 3：事件的节流（throttle）与防抖（debounce）

有些浏览器事件可以在短时间内快速触发多次，比如调整窗口大小或向下滚动页面。例如，监听页面窗口滚动事件，并且用户持续快速地向下滚动页面，那么滚动事件可能在 3 秒内触发数千次，这可能会导致一些严重的性能问题。

如果在面试中讨论构建应用程序，出现滚动、窗口大小调整或按下键等事件请务必提及 **防抖(Debouncing)** 和 **函数节流（Throttling）来提升页面速度和性能。这两兄弟的本质都是以闭包**的形式存在。通过对事件对应的回调函数进行包裹、以自由变量的形式缓存时间信息，最后用 setTimeout 来控制事件的触发频率。

**Throttle： 第一个人说了算**

throttle 的主要思想在于：在某段时间内，不管你触发了多少次回调，都只认第一次，并在计时结束时给予响应。

这个故事里，‘裁判’ 就是我们的节流阀， 他控制参赛者吃东西的时机， “参赛者吃东西”就是我们频繁操作事件而不断涌入的回调任务，它受 “裁判” 的控制,而计时器，就是上文提到的以自由变量形式存在的时间信息，它是 “裁判” 决定是否停止比赛的依据，最后，等待比赛结果就对应到回调函数的执行。

总结下来，所谓的“节流”，是通过在一段时间内无视后来产生的回调请求来实现的。只要 裁判宣布比赛开始，裁判就会开启计时器，在这段时间内，参赛者就尽管不断的吃，谁也无法知道最终结果。

对应到实际的交互上是一样一样的：每当用户触发了一次 scroll 事件，我们就为这个触发操作开启计时器。一段时间内，后续所有的 scroll 事件都会被当作“参赛者吃东西——它们无法触发新的 scroll 回调。直到“一段时间”到了，第一次触发的 scroll 事件对应的回调才会执行，而“一段时间内”触发的后续的 scroll 回调都会被节流阀无视掉。

现在一起实现一个 throttle：

```js
// fn是我们需要包装的事件回调, interval是时间间隔的阈值
function throttle(fn, interval) {
  // last为上一次触发回调的时间
  let last = 0
  
  // 将throttle处理结果当作函数返回
  return function () {
      // 保留调用时的this上下文
      let context = this
      // 保留调用时传入的参数
      let args = arguments
      // 记录本次触发回调的时间
      let now = +new Date()
      
      // 判断上次触发的时间和本次触发的时间差是否小于时间间隔的阈值
      if (now - last >= interval) {
      // 如果时间间隔大于我们设定的时间间隔阈值，则执行回调
          last = now;
          fn.apply(context, args);
      }
    }
}

// 用throttle来包装scroll的回调
const better_scroll = throttle(() => console.log('触发了滚动事件'), 1000)

document.addEventListener('scroll', better_scroll)
```

**代码部署后可能存在的BUG没法实时知道，事后为了解决这些BUG，花了大量的时间进行log 调试，这边顺便给大家推荐一个好用的BUG监控工具 Fundebug。**

干货集合：

> https://github.com/qq449245884/xiaozhi

