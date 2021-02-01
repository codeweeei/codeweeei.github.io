---
title: Node入门篇——fs、path、url等核心模块的基本使用
date: 2021/1/30
tags: [node模块]
categories: Node
---

# Node 入门

## <b style="color:skyblue">一：node 基础知识</b>

- 简介：node 是一款软件，它的功能是解析并运行 js（ECMAScript + node 相关的 api）
- 小技巧：使用 node-dev 可以实时运行 js，先`npm install node-dev -g`全局安装一下
- node 是一款命令行应用
- 打开命令行的方式：
  1. 使用 win + r ，输入 cmd
  2. 按住 shift + 鼠标右键，松开 shift 键，即可出现`在此处打开 Powershell 窗口`的选项，打开命令行窗口就可以定位到我们当前的目录
- 常用的 cmd 命令：
  1. `dir`查看当前目录下的所有内容
  2. `cd`切换工作目录

## <b style="color:skyblue">二：node 环境搭建及 node 中运行 js 文件</b>

- node 环境搭建可查看相关教程：
  https://codeweeei.github.io/blog/hello-world/
- node 中 js 的运行
  - 浏览器中 js 的运行是需要依赖一份 html 文件的
  - 在 node 中，js 文件可以独立运行
    - 打开命令行窗后，并且把工作目录定位到要运行的 js 文件的目录
    - 在命令行中输入 node 要运行的 js 文件名
  - 技巧
    - 使用 tab 键可以帮我们自动补全目录名称（在使用 node 或者 cd 的时候都可以使用）
    - 使用上下键，可以查看历史命令

## <b style="color:skyblue">三：node 中的模块</b>

- 模块化编程：
  1. commonjs 规范（node.js）
  2. AMD、CMD 规范（浏览器端的 require.js）
- 关于模块化的总结：

  1. 能够使各个模块独立运行，且互相不影响
  2. 模块与模块之间应该一种统一的合作方式

- node 中的模块
  1. 一份 js 文件就是一个模块
  2. node 中规定使用 require 方法引入模块
  3. node 中规定使用 module.exports 去导出模块
- node 中的模块分类
  1. 自定义模块（我们自己创建的 js 文件）
  2. 核心模块（包含在 node 内部，安装 node 环境，那么这些模块就可以直接使用了，比如 fs）
  3. 第三方模块（需要安装，有个人创建并按照规范传到 node 包管理中心的模块）

## <b style="color:skyblue">四：fs 模块的使用（核心）</b>

- fs 是一个核心模块，主要功能是操作目录和文件
- 引入 fs 模块：

```js
//引入模块
const fs = require("fs")
```

- fs 上有相关的方法

> 关于文件的方法

1. `fs.readFileSync()`读取文件

   - 第一个参数为读取文件的相对路径
   - 第二个参数为`'utf-8'`
   - 返回的就是读取的内容（字符串或二进制）

2. `fs.writeFileSync()`写入文件
   - 第一个参数为写入的路径
   - 第二个参数为要写入的内容，会彻底覆盖原文件（字符串格式）
   - 第三个参数为`'utf-8'`
3. `fs.unlinkSync()`删除文件
   - 参数为要删除的文件的路径
4. `fs.existsSync()`判断资源（文件或目录）是否存在
   - 参数为要判断的文件的路径
   - 返回值为布尔值
   - 一般在删除文件之前做判断
5. `fs.statSync()`判断该资源是文件还是目录
   - 参数为要判断的路径
   - 返回值为一个对象
     - 对象中`isFile()`为 true 则为文件
     - 对象中`isDirectory`为 true 则为目录

> 关于目录的方法

1. 创建目录

   - `fs.mkdirSync('./files')`

2. 读取目录（一级目录）
   - `fs.readdirSync('./files')`返回的是一个数组`['内容名称（test.js）']`
   - 如果是空目录，则返回空数组

- <b style="color:#f42">注意 path 模块的使用——拼接路径的技巧</b>

```js
const path = require("path")
path.join("./css", "index.css")
// 按照路径的规则来拼接路径-> css\index.css  可拼接无数片段
```

- <b style="color:#f42">注意 node 的常量\_\_direname 的使用——获取绝对路径</b>
  - 出现在模块中时，代表的是当前文件所在的目录的绝对路径
- <b style="color:#f42">可以结合 path 与\_\_direname 两者来得到想要的文件的绝对路径</b>

3. 删除目录
   - `fs.rmdirSync('./files')`
   - 该方法只能删除空目录

## <b style="color:skyblue">五：导入导出细节和原理</b>

### <b style="color:pink">1.导入 require 细节</b>

1. require 是 node 环境中提供的一个方法，作用是引入模块（在浏览器环境是没有该方法的）
2. require 函数接受的参数是字符串形式的（路径或模块名）
3. 路径分为相对路径和绝对路径

   - 绝对路径一般都是从盘符开始，使用\_\_direname 和 path.join 来获取
   - 相对路径

     - 如果是 require 引用路径那么相对路径的参照目录就永远是当前调用 require 方法的文件所在的目录
     - 其他相对路径的参考目录永远都是工作目录（命令行所在的目录，工作目录是可变的，所以尽量使用绝对路径）

4. 如果接受的参数是模块名的话，那么引用的就不是模块（文件）而是一个“包”，就会按照包的规则来加载

### <b style="color:pink">2.导出 export 细节跟原理</b>

1. 导出的细节
   - Module.exports 是 node 环境提供的一个对象，作用是导出模块（在浏览器中是没有该对象的）

```js
//module.exports应该是如下
const module = {
  exports: {
    //
  },
}
```

2. node 中其实还存在一个对象 exports 可以导出对象
   - `module.exports === exports` 为 true，即代表这两个地址指向同一个对象
   - 但是在真正导出时，识别的是 module.exports

```js
var a = 1
var module = {
  exports: {
    //
  }
}
exports = module.exports{}
module.exports = a//可以导出a，require返回的是1
exports.a = a//可以导出a，require返回的是{a:1}
module.exports.a = a//可以导出a,，require返回的是{a:1}
exports = a //不可导出a
```

3. 导出多个内容
   - `module.exports = {add,sub}`
   - `exports.add = add;exports.sub = sub`

## <b style="color:skyblue">六：node 中的包</b>

1. 包就是把多个 node 模块集合在一起，但是需要遵循相关的规则，一个集合了 js 文件（入口文件）的目录就是一个包
2. 包相关的规则：在项目目录根部创建`package.json`文件，该文件中有众多属性，这些属性赋予了包额外的特性，一般使用 `npm init` 创建，`main`属性表示入口文件。
3. 引入包
   - 在 require 方法中只需要写入包的路径，在包中会默认查找`index.js`文件，如果没有，则会查找 `package.json` 中的 main 字段对应的文件，如果都没有则会报错
   - `const xxx = require('./xxx')`
4. 以包名引入包
   - 例如 fs 模块
   - 包必须存入 `node_modules` 文件夹中
   - 包必须符和包的规范
5. 通过包名去查找包的一些规则
   - 在当前目录下 node_modules 下查找包名的目录
   - 按照包的规则去加载包（同上）
   - 如果没找到对应的目录，继续向上一层按照该规则去查找，直到找到盘符的根目录 C/,D/
   - 如果在根目录还没找到，那么就会去找全局环境变量 NODE_PATH 中查找

## <b style="color:skyblue">六：npm 的初步使用</b>

- npm 介绍：npm 属于 node 的一个模块，node package manger（node 的包管理器）
- 通过命令行使用 （npm install 包名）
  - 从 npm 官网里安装
  - 详细安装及配置可查看：https://codeweeei.github.io/blog/hello-world/

### <b style="color:pink">1.npm 管理包的一些命令</b>

1. 创建包的描述文件 `package.json(npm init)`
2. 安装包的命令 （npm install 包名）
   1. 从 npm 的官方网站下载
   2. 安装到指定的位置（分全局安装和本地安装）
   3. 全局安装 `npm install packageName -g`
   4. 本地安装（生产环境） `npm install packageName --save` /`npm install packageName -S`
   5. 本地安装（开发环境） `npm install packageName --save-dev` / `npm install packageName -D`
      - 本地安装的包被安装在工作目录的 node_modules 下
      - 生产环境（上线后需要）的包在 package.json 里的"dependencies"字段里添加依赖
      - 开发环境（上线后不需要）的包在 package.json 里的"devDependencies"字段里添加依赖
3. 快速安装（`npm install`）
   1. 在有 package.json 的工作目录下执行
   2. 检测 package.json 中的 depdencies 和 devDepdencies 两个字段，并且全部安装它们所保存的包
4. 卸载包
   - `npm uninstall packageName`
5. 设置和查看全局包的安装位置的命令
   - `npm config get prefix`
   - `npm config set prefix 新的目录地址`
6. 设置和查看我们的下载源
   - `npm config get registry`
   - `npm config set registy 下载地址`
   - 淘宝的镜像地址 https://registry.npm.taobao.org/

## <b style="color:skyblue">七：node 环境中如何运行 es6 模块语法</b>

- 使用 babel 转码成 es5，但是它这针对 js 文件，其他类型无法迁移
  node 环境几乎支持除了 es 模块之外所有的语法，babel 提供了一个`@babel/register` 的模块
  该模块的作用是在运行时解决 node 不支持 es6 的问题
- 使用用法：
  在运行入口文件的时候，添加一个启动模块（entry.js),写入以下内容

```js
require("@babel/register")({
  //在运行时，会去转码对应的es6模块语法为commonjs语法
  plugins: ["@babel/plugin-transform-modules-commonjs"],
})
// 加载目标文件
require("./index")
```

- 在 node 中运行 entry.js 即可

## <b style="color:skyblue">八：node 中引入 url 模块及 querystring 模块</b>

- url 地址详解
  - 可参见 https://codeweeei.github.io/blog/url%E5%9C%B0%E5%9D%80%E8%AF%A6%E8%A7%A3/

### 例子 1：`url.parse('url 字符串')`解析 url 路径——url 模块

```js
const url = require("url")
//符和url地址规则的字符串
const urlStr =
  "http://140.143.201.230:8888/public/course/course?id=123&pwd=45678#home"
//将字符串解析为url对象
const oUrl = url.parse(urlStr)

console.log(oUrl)
```

运行结果

```js
Url {
  protocol: 'http:',
  slashes: true,
  auth: null,
  host: '140.143.201.230:8888',
  port: '8888',
  hostname: '140.143.201.230',
  hash: '#home',
  search: '?id=123&pwd=45678',
  query: 'id=123&pwd=45678',
  pathname: '/public/course/course',
  path: '/public/course/course?id=123&pwd=45678',
  href: 'http://140.143.201.230:8888/public/course/course?id=123&pwd=45678#home'
}
```

### 例子 2：`url.parse('url 字符串')`解析 查询字符串 query——querystring 模块

```js
const url = require("url")
//引入querystring模块
const qs = require("querystring")
//符和url地址规则的字符串
const urlStr =
  "http://140.143.201.230:8888/public/course/course?id=123&pwd=45678#home"

const oUrl = url.parse(urlStr)

const oQs = qs.parse(oUrl.query)
console.log(oQs)
```

运行结果

```js
{
  id:'123',
  pwd:'45678'
}
```
