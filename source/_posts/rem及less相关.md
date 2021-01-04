---
title: rem及less相关
date: 2021/1/4
tags: [less]
categories: CSS
---

## <b style="color:pink">rem 基础</b>

### 定义：

- rem (root em)是一个相对单位，类似于 em，em 是相对于父元素字体大小。不同的是 rem 的基准是相对于 html 元素的字体大小。即相对于 html 标签里设置的 font-size 大小。

### 优点：

- 可以通过改变 html 标签里的文字大小来改变页面中元素的大小以达到整体控制；

## <b style="color:pink">媒体查询</b>

### 定义：媒体查询（Media Query）是 css3 的新语法；

### 作用：

- 使用@media 可以根据不同的媒体类型来定义不同的样式；
- @media 可针对不同屏幕尺寸来设置不同的样式；
- 重置浏览器大小时，页面也会根据浏览器的宽度和高度重新渲染页面；

### 代码规范：

```cs
@media mediatype and | not | only (media feature) {
  CSS-Code;
}
```

### 例子：

```cs
  // 即当屏幕宽度小于800px时，body的背景是蓝色
  @media screen and (min-width: 800px) and (max-width: 1200px) {
    body {
      background:blue;
    }
  }
```

### 媒体查询引入资源

- 解释：当样式比较繁多时，可以针对不同的媒体使用不同的 stylesheets（样式表）；
- 原理：直接在 link 中判断设备的尺寸，然后引入不同的 css 文件；
- 语法规范：

```html
<link
  rel="stylesheet"
  media="mediatype and|not|only (media feature)"
  href="xxx.css"
/>
```

## <b style="color:pink">less 基础</b>

### css 的弊端

- 亢余度很高；
- 不利于维护；
- 不利于复用；
- 不利于计算；

### 定义

- 用于维护 css 的弊端，less 是一门 css 扩展语言，也称为 css 预处理器；在 css 语法基础上，引入了变量和，Mixin，运算以及函数等功能，大大简化了 css 的编写；
- Less 中文网址
  > [http://lesscss.cn/](http://lesscss.cn/)
- 常见的 css 预处理器：Sass、Less、Stylus

## <b style="color:pink">less 的使用</b>

- 新建一个后缀名为.less 的文件

### less 变量

- 变量是指没有固定的值，可以改变的，在 css 中的颜色和数值经常使用；
- 语法：<span style="color:#51bfad">@ 变量名: 值 ;</span>
- 变量名规范
  - 必须有@前缀
  - 不能包含特殊字符
  - 不能以数字开头
  - 大小写敏感
- 变量的使用：

```less
//定义一个蓝色的变量
@color: blue;
div {
  color: @color;
}
```

### less 编译

- 定义：本质上，less 包含一套自定义的语法及一个解析器，用户根据这些语法定义自己的样式规则，这些规则最终会通过解析器，编译生成对应的 css 文件；所以，我们需要把我们的 less 文件，编译生成为 css 文件，这样 html 文件才可使用。
- vscode 插件：<span style="color:#51bfad">Easy Less</span>
  - 将 less 文件编译成 css 文件；
  - 安装完插件后，重新加载下 vscode，只要 ctrl + s 保存下 less 文件，会自动生成 对应的 css 文件；

### less 嵌套

- 定义：子元素的样式直接写在父元素的里面即可；
- 特殊：如果遇到（交集|伪类|伪元素选择器）
  - 内层选择器的前面没有&符号，则它被解析为父选择器的后代；
  - 如果有&符号，它就被解析为父元素自身或父元素的伪类；

### less 运算

- 任何数字和变量都可以参与运算

```less
@width: 10px + 5;
div {
  border: @width solid red;
}
//生成的css文件
div {
  border: 15px solid red;
}
```

- 注意事项
  - 乘法（\*）和除号（/）的写法
  - <span style="color:#51bfad">运算符中间左右有个空格隔开</span>
  - 对于两个不同的单位的值之间的运算，运算结果取第一个值得单位
  - 如果两个值之间只有一个值有单位，则运算结果就取该单位

## <b style="color:pink">rem 的适配方案</b>

- 让一些不能等比例自适应的元素，达到设备尺寸发生改变的时候，等比例适配当前设备；
- 使用媒体查询根据不同设备按比例设置 html 里文字的大小，然后页面里使用 rem 作为单位，从而达到适配当前设备；

### 最终适配方案

1. 首先选一套标准尺寸（750px）；
2. 用屏幕尺寸除以划分的份数（15），得到了 HTML 里的文字大小，但注意不同屏幕下得到的 html 文字大小不一样；
3. 页面元素的 rem 值=页面元素在 750px 下的 px 值/HTML 里面的文字大小；
