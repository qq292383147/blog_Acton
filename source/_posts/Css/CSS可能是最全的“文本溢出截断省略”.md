---
title: CSS可能是最全的“文本溢出截断省略”
date: 2019-11-13 22:00:00
type: css
tags: css
categories: css
---

## 先来点基础的，单行文本溢出省略

**核心 CSS 语句**

- <font color="#e0246a">overflow</font>: <font color="#5cc4d8">hidden</font>；（文字长度超出限定宽度，则隐藏超出的内容）
- <font color="#e0246a">white-space</font>:<font color="#5cc4d8">nowrap</font>；（设置文字在一行显示，不能换行）
- <font color="#e0246a">text-overflow</font>: <font color="#5cc4d8">ellipsis</font>；（规定当文本溢出时，显示省略符号来代表被修剪的文本）

**优点**

- 无兼容问题
- 响应式截断
- 文本溢出范围才显示省略号，否则不显示省略号
- 省略号位置显示刚好

**短板**

- 只支持单行文本截断

**适用场景**

- 适用于单行文本溢出显示省略号的情况

**Demo**

```html
<style>
    .demo {
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
    }
</style>
<body>
	<div class="demo">这是一段很长的文本</div>
</body>
```

**示例图片**

![98](http://zhanglong292383147.gitee.io/picture_images/picture/css/98.gif)

## 进阶一下，多行文本溢出省略（按行数）

### ○ 纯 CSS 实现方案

**核心 CSS 语句**

- <font color="#e0246a">-webkit-line-clamp</font>: <font color="#5cc4d8">2</font>；（用来限制在一个块元素显示的文本的行数, 2 表示最多显示 2 行。 为了实现该效果，它需要组合其他的WebKit属性）
- <font color="#e0246a">display</font>: <font color="#5cc4d8">-webkit-box</font>；（和 1 结合使用，将对象作为弹性伸缩盒子模型显示 ）
- <font color="#e0246a">-webkit-box-orient</font>: <font color="#5cc4d8">vertical</font>；（和 1 结合使用 ，设置或检索伸缩盒对象的子元素的排列方式 ）
- <font color="#e0246a">overflow</font>: <font color="#5cc4d8">hidden</font>；（文本溢出限定的宽度就隐藏内容）
- <font color="#e0246a">text-overflow</font>: <font color="#5cc4d8">ellipsis</font>；（多行文本的情况下，用省略号“…”隐藏溢出范围的文本)

**优点**

- 响应式截断
- 文本溢出范围才显示省略号，否则不显示省略号
- 省略号显示位置刚好

**短板**

- 兼容性一般： -webkit-line-clamp 属性只有  WebKit  内核的浏览器才支持
- ![99](http://zhanglong292383147.gitee.io/picture_images/picture/css/99.jpg)

**适用场景**

- 多适用于移动端页面，因为移动设备浏览器更多是基于 WebKit 内核

**Demo**

```html
<style>
	.demo {
		  display: -webkit-box;
	    overflow: hidden;
	    -webkit-line-clamp: 2;
	    -webkit-box-orient: vertical;
	}
</style>

<body>
	<div class='demo'>这是一段很长的文本</div>
</body>
```

**示例图片**

![100](http://zhanglong292383147.gitee.io/picture_images/picture/css/100.gif)

### ○ 基于 JavaScript 的实现方案

**优点**

- 无兼容问题
- 响应式截断
- 文本溢出范围才显示省略号，否则不显示省略号

**短板**

- 需要 JS 实现，背离展示和行为相分离原则
- 文本为中英文混合时，省略号显示位置略有偏差

**适用场景**

- 适用于响应式截断，多行文本溢出省略的情况

**Demo**

当前仅适用于文本为中文，若文本中有英文，可自行修改

```html
<script type="text/javascript">
    const text = '这是一段很长的文本';
    const totalTextLen = text.length;
    const formatStr = () => {
        const ele = document.getElementsByClassName('demo')[0];
        const lineNum = 2;
        const baseWidth = window.getComputedStyle(ele).width;
        const baseFontSize = window.getComputedStyle(ele).fontSize;
        const lineWidth = +baseWidth.slice(0, -2);

        // 所计算的strNum为元素内部一行可容纳的字数(不区分中英文)
        const strNum = Math.floor(lineWidth / +baseFontSize.slice(0, -2));

        let content = '';
        
      	// 多行可容纳总字数
        const totalStrNum = Math.floor(strNum * lineNum);

        const lastIndex = totalStrNum - totalTextLen;

        if (totalTextLen > totalStrNum) {
            content = text.slice(0, lastIndex - 3).concat('...');
        } else {
            content = text;
        }
        ele.innerHTML = content;
    }
    
    formatStr();
    
		window.onresize = () => {
        formatStr();
    };
</script>

<body>
	<div class='demo'></div>
</body>
```

**示例图片**

![101](http://zhanglong292383147.gitee.io/picture_images/picture/css/101.gif)

## 再进阶一步，多行文本溢出省略（按高度）

### ○ 多行文本溢出不显示省略号

**核心 CSS 语句**

- <font color="#e0246a">overflow</font>: <font color="#5cc4d8">hidden</font>；（文本溢出限定的宽度就隐藏内容）
- <font color="#e0246a">line-height</font>: 20<font color="#5cc4d8">px</font>；（结合元素高度，高度固定的情况下，设定行高， 控制显示行数）
- <font color="#e0246a">max-height</font>: 40<font color="#5cc4d8">px</font>；（设定当前元素最大高度）

**优点**

- 无兼容问题
- 响应式截断

**短板**

- 单纯截断文字, 不展示省略号，观感上较为生硬

**适用场景**

- 适用于文本溢出不需要显示省略号的情况

**Demo**

```html
<style>
	.demo {
		overflow: hidden;
		max-height: 40px;
		line-height: 20px;
	}
</style>

<body>
	<div class='demo'>这是一段很长的文本</div>
</body>
```

**示例图片**

![102](http://zhanglong292383147.gitee.io/picture_images/picture/css/102.gif)

### ○ 伪元素 + 定位实现多行省略

**核心 CSS 语句**

- position: relative; （为伪元素绝对定位）
- overflow: hidden; （文本溢出限定的宽度就隐藏内容）
- position: absolute;（给省略号绝对定位）
- line-height: 20px; （结合元素高度,高度固定的情况下,设定行高, 控制显示行数）
- height: 40px; （设定当前元素高度）
- ::after {}  （设置省略号样式）

**优点**

- 无兼容问题
- 响应式截断

**短板**

- 无法识别文字的长短，无论文本是否溢出范围, 一直显示省略号
- 省略号显示可能不会刚刚好，有时会遮住一半文字

**适用场景**

- 适用于对省略效果要求较低，文本一定会溢出元素的情况

**Demo**

```html
<style>
    .demo {
        position: relative;
        line-height: 20px;
        height: 40px;
        overflow: hidden;
    }
    .demo::after {
        content: "...";
        position: absolute;
        bottom: 0;
        right: 0;
        padding: 0 20px 0 10px;
    }
</style>

<body>
	<div class='demo'>这是一段很长的文本</div>
</body>
```

**示例图片**

![103](http://zhanglong292383147.gitee.io/picture_images/picture/css/103.gif)

### ○ 利用 Float 特性，纯 CSS 实现多行省略

**核心 CSS 语句**

- line-height: 20px；（结合元素高度,高度固定的情况下,设定行高, 控制显示行数）
- overflow: hidden；（文本溢出限定的宽度就隐藏内容）
- float: right/left；（利用元素浮动的特性实现）
- position: relative；（根据自身位置移动省略号位置, 实现文本溢出显示省略号效果）
- word-break: break-all；（使一个单词能够在换行时进行拆分）

**优点**

- 无兼容问题
- 响应式截断
- 文本溢出范围才显示省略号，否则不显示省略号

**短板**

- 省略号显示可能不会刚刚好，有时会遮住一半文字

**适用场景**

- 适用于对省略效果要求较低，多行文本响应式截断的情况

**Demo**

```html
<style>
    .demo {
        background: #099;
        max-height: 40px;
        line-height: 20px;
        overflow: hidden;
    }
    .demo::before{
        float: left;
        content:'';
        width: 20px;
        height: 40px;
    }

    .demo .text {
        float: right;
        width: 100%;
        margin-left: -20px;
        word-break: break-all;
    }
    .demo::after{
        float:right;
        content:'...';
        width: 20px;
        height: 20px;
        position: relative;
        left:100%;
        transform: translate(-100%,-100%);
    }
</style>

<body>
    <div class='demo'>
    	<div class="text">这是一段很长的文本</div>
    </div>
</body>
```

**示例图片**

![104](http://zhanglong292383147.gitee.io/picture_images/picture/css/104.gif)

**原理讲解**

有 A、B、C 三个盒子，A 左浮动，B、C 右浮动。设置 A 盒子的高度与 B 盒子高度（或最大高度）要保持一致

1. 当的 B 盒子高度低于 A 盒子，C 盒子仍会处于 B 盒子右下方。
2. 如果 B 盒子文本过多，高度超过了 A 盒子，则 C 盒子不会停留在右下方，而是掉到了 A 盒子下。
3. 接下来对 C 盒子进行相对定位，将 C 盒子位置向右侧移动 100%，并向左上方向拉回一个 C 盒子的宽高（不然会看不到哟）。这样在文本未溢出时不会看到 C 盒子，在文本溢出时，显示 C 盒子。

![105](http://zhanglong292383147.gitee.io/picture_images/picture/css/105.gif)

## 收，大道归简，能力封装

> 凡重复的，让它单一；凡复杂的，让它简单。

每次都要搞一坨代码，太麻烦。这时候你需要考虑将文本截断的能力，封装成一个可随时调用的自定义容器组件。市面上很多 UI 组件库，都提供了同类组件的封装，如基于 Vue 的 ViewUI Pro，或面向小程序提供组件化解决能力的 MinUI 。

![106](http://zhanglong292383147.gitee.io/picture_images/picture/css/106.jpg)

![107](http://zhanglong292383147.gitee.io/picture_images/picture/css/107.jpg)

