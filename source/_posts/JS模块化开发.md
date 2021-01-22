---
title: JS模块化开发
date: 2021/1/8
tags: [js模块化]
categories: Javascript
---

# 模块化开发

> 为什么要有模块化？

- 随着 Ajax 异步请求的出现，慢慢形成了前后端的分离；
- 客户端的工作量就日益渐增，代码量也与之同时增加，为了应付代码量的剧增，我们通常需要将代码组织在多个 js 文件中，进行维护；
- 但是这种维护方式会出现很多灾难性的问题：
  - 比如全局变量同名的问题；
  - 代码的编写方式对 js 文件的依赖顺序几乎是强制的；

## <b style="color:skyblue">一：前端模块化雏形</b>

### _1.使用匿名函数_

- 可以使用将需要暴露到外面的变量，使用一个模块作为出口
- 在匿名函数内部，定义一个对象，给对象添加各种需要暴露到外面的属性和方法(不需要暴露的直接定义即可)。最后将这个对象返回，并且在外面使用了一个 MoudleA 接收。

```js
var MoudleA = (function () {
  //1.定义一个对象
  var obj = {}
  //2.在对象内部添加变量和方法
  obj.flag = true
  obj.myFunc = function (info) {
    //...
  }
  //3.将对象返回
  return obj
})()
```

### _2.常见的模块化规范_

- 目前模块化开发已经有了很多既有的规范，以及对应的实现方案。
- CommonJS(常用)、 AMD、 CMD、 ES6 的 Modules（常用）

## <b style="color:skyblue">二：commonJS</b>

### _1.规范定义_

- Nodejs 模块系统遵循此规范，适用于服务端
- CommonJS 规范规定，一个文件就是一个模块，用 module 变量代表当前模块。 Node 在其内部提供一个 Module 的构建函数。所有模块都是 Module 的实例；
- 模块化的两个核心：导入和导出;

### _2.commonJS 的导出_

- module.exports 属性表示当前模块对外输出的接口，其他文件加载该模块，实际上就是读取 module.exports 变量。

```js
//moduleA.js
module.exports = {
  flag: true,
  test(a, b) {
    return a + b
  },
}
```

### _3.commonJS 的导入_

- require 函数的基本功能是，读入并执行一个 JavaScript 文件，然后返回该模块的 exports 对象。

```js
let { test, flag } = require("moduleA")
//等价于
let _mA = require("moduleA")
let test = _mA.test
let flag = _mA.flag
```

## <b style="color:skyblue">三：ES6 的 Modules</b>

- 使用 export 指令导出了模块对外提供的接口，之后我们就可以使用 import 命令来加载对应的这个模块；
- 首先需要在 HTML 代码中，引入这两个文件，并且类型需要设置为 module

```html
<script src="info.js" type="module"></script>
<script src="test.js" type="module"></script>
```

### _1.export 的基本使用_

```js
//info.js
export let name = "cw"
export let age = 22
export let height = 200
//另一种写法
export { name, age, height }
```

- export 导出函数或类

```js
//之前是输出变量，也可以用来导出函数或类
function test(content) {
  console.log(content)
}
class Person {
  constructor(name, age) {
    this.name = name
    this.age = age
  }
  eating() {
    console.log("干饭人，干饭魂")
  }
}
//一起导出
export { test, Person }
```

- export default
  - 当某种情况下，一个模块包含某种功能，我们不希望给这个功能命名，而且让导入者可以自己来命名，这时候就可以使用 export default
  - <b style="color:pink">注意：export default 在同一模块不允许同时存在多个；</b>
  ```js
  //在info.js中导出
  export default function () {
    console.log("default function")
  }
  //在test.js中导入
  import myFunc from "./info.js"
  myFunc()
  ```

### _2.import 的基本使用_

```js
//test.js
//当直接使用export 导出时
import { name, age, height } from "./info.js"
//当使用export default导出时
import myFunc from "./info.js"
//如果希望将某模块中导出的所有内容都导入，可以使用*来导入模块中导出的所有变量，通常需要给*一个别名，方便后续使用
import * as info from "./info.js"
console.log(info.name, info.age, info.height)
```
