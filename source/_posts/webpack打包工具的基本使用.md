---
title: webpack打包工具的基本使用
date: 2021/1/10
tags: [webpack]
categories: 前端工具
---

# webpack 详解

## <b style="color:skyblue">一：认识 webpack</b>

- webpack 是一个现代的 Javascript 应用的静态<b style="color:#f1c3b8">模块打包</b>工具;
- webpack 需要依赖 <b style="color:#f1c3b8">node</b> 环境才可以正常运行；
- webpack 编译代码的所有方法:在 webpack 打包应用程序时，可以选择各种模块语法风格，包括 ES6(推荐), CommonJS 和 AMD。
- webpack 中模块化的概念：
  - 模块化开发完成了项目后，还需要处理模块间的各种依赖，并且将其进行整合打包，而 webpack 其中一个核心就是让我们可以进行模块化开发，并且会帮助我们处理模块间的依赖关系。
  - 而且不仅仅是 JavaScript 文件，我们的 CSS、图片、json 文件等等在 webpack 中都可以被当做模块来使用。
- webpack 中打包的概念：
  - 将 webpack 中的各种资源模块进行打包合并成一个或多个包(Bundle)。
  - 并且在打包的过程中，还可以对资源进行处理，比如压缩图片，将 scss 转成 css，将 ES6 语法转成 ES5 语法，将 TypeScript 转成 JavaScript 等等操作。
- webpack 与 gulp、grunt 打包工具的区别
  - gulp、grunt 更强调的是<b style="color:#f1c3b8">前端流程的自动化</b>，模块化不是它们的核心；
  - webpack 更加强调<b style="color:#f1c3b8">模块化开发管理</b>，而且提供文件压缩合并，预处理等功能

## <b style="color:skyblue">二：webpack 的安装</b>

### <b style="color:pink">_1.node.js 的安装_</b>

- 安装 webpack 首先需要[安装 Node.js](https://codeweeei.github.io/blog/hello-world/)，Node.js 自带了软件包管理工具 npm；

### <b style="color:pink">_2.安装 webpack_</b>

- 全局安装：(指定安装 3.6.0 的版本，因为 vue cli2 依赖该版本)

```cmd
npm install webpack@3.6.0 -g
```

- 局部安装：（后续需要）
  - 为什么需要局部安装：在终端直接执行 webpack 命令，使用的全局安装的 webpack ，当在 package.json 中定义了 scripts 时，其中包含了 webpack 命令，那么使用的是局部 webpack

```cmd
  cd 对应目录
  npm install webpack@3.6.0 --save-dev
  --save-dev 是开发时依赖，项目打包后是不需要继续使用的。
```

## <b style="color:skyblue">三：webpack 的起步</b>

### <b style="color:pink">_1.准备工作_</b>

- 创建 dist 文件夹：用于存放打包之后的文件；
  - bundle.js：打包后的存放目标文件（自动创建）；
- 创建 src 文件夹：用于存放写的源代码；
  - main.js：打包时的入口文件；(依赖于 test.js 文件，使用 commonJS 代码引用了 test.js 文件)
  - test.js：测试 js 文件；
- index.html：浏览器打开展示的首页 html；
- package.json：通过 npm init 生成的，npm 包管理的文件；

### <b style="color:pink">_2.js 文件的打包_</b>

- index.js 不能直接引用 main.js 文件，因为浏览器不识别 commonJS 代码，此时需要使用 webpack 将 main.js 打包生成一个最终的 js 文件（会自动处理该文件的依赖关系）
- 使用：`$webpack ./src/main.js ./dist/bundle.js`终端命令即可打包 main.js 文件并且生成在 dist 文件夹里的 bundle.js（自动创建）

## <b style="color:skyblue">四：webpack 的配置</b>

### <b style="color:pink">_1.配置出口入口文件_</b>

- 之前打包需要写很长的命令行进行打包，这样过于繁琐，可以配置下出口入口文件，之后就只需输入`webpack`即可进行打包（但是后续一般是使用 npm run build 来进行打包，后续会讲）；
- 在根目录创建 `webpack.config.js` 文件

```js
//webpack.config.js

//从node包里依赖path
const path = require("path")

module.exports = {
  entry: "./src/main.js",
  //output 属性告诉 webpack 在哪里输出它所创建的 bundles，以及如何命名这些文件，默认值为 ./dist
  output: {
    //需要动态获取path绝对路径：__dirname用于获取当前上下文路径，后面拼接dist
    path: path.resolve(__dirname, "dist"),
    filename: "bundle.js",
  },
}
```

## <b style="color:skyblue">_五：loader_</b>

- 介绍：loader 让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）。loader 可以将所有类型的文件（.css/.scss/.vue/ES6 语法等）转换为 webpack 能够处理的有效模块，然后你就可以利用 webpack 的打包能力，对它们进行处理。
- 使用过程：
  - 通过 npm 安装需要使用的 loader；
  - 在`webpack.config.js`配置文件里扩展对应的 loader 即可；

### <b style="color:pink">_1.代码示例一：css 文件处理_</b>

- 安装需要的 loader

```js
npm install --save-dev css-loader
//css-loader解析css文件后，使用@import加载，并且返回css代码；
npm install --save-dev style-loader
//style-loader将模块导出并将样式添加到dom结构中
```

- 配置方法

```js
//webpack.config.js
module.exports = {
  module: {
    rules: [
      {
        //正则匹配所有以.css结尾的文件
        test: /\.css$/,
        //注意使用多个loader时是从右向左顺序的
        use: ["style-loader", "css-loader"],
      },
    ],
  },
}
```

### <b style="color:pink">_2.代码示例二：less 文件处理_</b>

- 安装需要的 loader

```bash
npm install --save-dev less-loader less
```

- 配置方法

```js
// webpack.config.js
module.exports = {
    ...
    module: {
        rules: [{
            test: /\.less$/,
            use: [{
                loader: "style-loader" // 3.creates style nodes from JS strings
            }, {
                loader: "css-loader" // 2.translates CSS into CommonJS
            }, {
                loader: "less-loader" // 1.compiles Less to CSS
            }]
        }]
    }
};
```

### <b style="color:pink">_3.代码示例三：图片文件处理_</b>

- 安装需要的 loader

```bash
npm install --save-dev url-loader
npm install --save-dev file-loader
```

- 配置方法

```js
// webpack.config.js
module.exports = {
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "bundle.js",
    //publicPath可以设置关于url的路径前面加上dist/
    publicPath: "dist/",
  },
  module: {
    rules: [
      {
        test: /\.(png|jpg|gif|jpeg)$/,
        use: [
          {
            loader: "url-loader",
            options: {
              //当加载的图片，小于limit时，会将图片编译成base64字符串形式
              //当加载的图片，大于limit时，需要使用file-loader模块进行加载
              limit: 8192,
              //img：文件要打包到的文件夹;name：获取图片原来的名字，放在该位置;hash:8：为了防止图片名称冲突，依然使用hash，但是我们只保留8位;ext：使用图片原来的扩展名
              name: "img/[name].[hash:8].[ext]",
            },
          },
        ],
      },
    ],
  },
}
```

- 注意事项
  - 图片大小大于 limit 时需要下载 file-loader；
  - 图片修改文件名称：webpack 会自动帮助我们生成一个非常长的名字：32 位的 hash 值，目的防止名字重复；但真实开发中，可能需要对打包的名字有一定的要求；就需要在 options 里配置 `name: "img/[name].[hash:8].[ext]"`
  - 图片使用的路径问题：默认情况下，webpack 会将生成的路径直接返回给使用者，整个程序打包在 dist 文件夹下的，所以需要在路径上再添加 dist/；

### <b style="color:pink">_4.代码示例四：ES6 语法处理_</b>

- webpack 默认打包时不会将 ES6 的语法转成 ES5 代码，而很多浏览器对于 ES6 还不支持，所以就需要将 ES6 转换成 ES5 代码；
- 安装需要的 loader

```bash
npm install --save-dev babel-loader@7 babel-core babel-preset-es2015
```

- 配置方法

```js
  {
    test: /\.js$/,
    exclude: /(node_modules|bower_components)/,
    use: {
      loader: 'babel-loader',
      options: {
        presets: ['es2015']
      }
    }
  }
```

## <b style="color:skyblue">_六：plugin_</b>

- 简介：plugin 是插件的意思，就是对 webpack 现有功能的扩展，比如打包优化，文件压缩等；
- 与 loader 的区别
  - loader 主要是用于转换加载某些类型的模块；
  - plugin 主要是对 webpack 本身的扩展。
- 使用过程
  - 通过 npm 安装需要的 plugins，某些 webpack 内置的 plugins 不需要安装；
  - 在 webpack.config.js 中的 plugins 进行配置。

### <b style="color:pink">_1.代码示例一：添加版权的 plugin_</b>

- `webpack.config.js`

```js
const path = require("path")
const webpack = require("webpack")

module.exports = {
  ...
  plugins: [
    //内置的plugin
    new webpack.BannerPlugin('最终版权归xxx')
  ]
}
```

### <b style="color:pink">_2.代码示例二：打包 html 的 HtmlWebpackPlugin 插件_</b>

- 目前 index.html 文件是存放在项目的根目录下的，默认情况下真实发布项目时，发布的是 dist 文件夹中的内容，但是 dist 文件夹中如果没有 index.html 文件夹的话，那么打包的 js 文件就没有意义了；
- HtmlWebpackPlugin 插件可以自动生成 index.html 文件（指定模板来生成），将打包的 js 文件自动通过 script 标签来插入到 body 中。
- 安装：`npm install html-webpack-plugin --save-dev`
- `webpack.config.js`

```js
const path = require("path")
const HtmlWebpackPlugin = require("html-webpack-plugin")

module.exports = {
  ...
  plugins: [
    new HtmlWebpackPlugin({
      //这里插件会根据template的模板来生成相似的index.html
      template: 'index.html'
    })
  ]
}
```

- 注意事项：publicPath 会改变 src 引入的路径，注意取消 publicPath 的设置；

### <b style="color:pink">_3.代码示例三：压缩 js 文件的 uglify-js-webpack-plugin 插件_</b>

- 对打包的 js 文件进行压缩丑化；
- 安装插件：`npm i -D uglifyjs-webpack-plugin`
- 最后打包后的 bundle.js 就是已经压缩后的文件
- `webpack.config.js`

```js
const UglifyJsPlugin = require("uglifyjs-webpack-plugin")

module.exports = {
  plugins: [new UglifyJsPlugin()],
}
```

## <b style="color:skyblue">七：搭建本地服务器自动打包运行 webpack-dev-serve </b>

- 简介：webpack 提供了一个可选的本地开发服务器，这个本地服务器基于 node.js 搭建，内部使用 express 框架，可以实现我们想要的让浏览器自动刷新显示我们修改后的结果。
- 安装模块：`npm install --save-dev webpack-dev-server@2.9.1 `
- `webpack.config.js`
- devServe 作为 webpack 中的一个选项，可以设置如下属性：
  - contentBase：为哪一个文件夹提供本地服务，默认是根文件夹（./dist）
  - port：端口号（默认端口为 8080）
  - inline：实时刷新
  - historyApiFallback：在 SPA（单页面富应用）中，依赖 HTML5 的 history 模式

```js
devServe: {
  contentBase: './dist',
  inline: true
}
```

- 默认使用 `webpack-dev-serve`命令来跑项目 ，为了方便的话也可以配置一个 scripts 来运行
  - open 参数表示直接打开浏览器

```js
"dev":"webpack-dev-serve" --open
```

## <b style="color:skyblue">八：webpack 配置文件的分离</b>

### <b style="color:pink">_1.分离公共配置文件在 built 文件夹中_</b>

- `base.config.js`

```js
const path = require("path")

module.exports = {
  entry: {
    path: "./src/main.js",
  },
  output: {
    //注意输出的路径是相对该文件的路径的，为了生成dist文件夹在根目录下，需要在前面加../
    path: path.resolve(__direname, "../dist"),
    filename: "bundle.js",
  },
}
```

### <b style="color:pink">_2.分离开发时依赖在 built 文件夹中_</b>

- `dev.config.js`

```js
cosnt devServe = require('webpack-dev-serve')
cosnt baseConfig = require('./base.config')
cosnt webpackMerge = require ('webpack-merge')
//合并公共配置文件跟开发时配置文件
module.exports = webpackMerge(baseConfig,{
  devServe:{
    contentBase: './dist',
    inline: true
  }
})
```

### <b style="color:pink">_3.分离生产时依赖在 built 文件夹中_</b>

- `prod.config.js`

```js
const uglifyJsWebpackPlugin = require ('uglifyjs-webpack-plugin')
cosnt webpackMerge = require ('webpack-merge')
cosnt baseConfig = require('./base.config')
//合并公共配置文件跟生产时配置文件
module.exports = webpackMerge(baseConfig,{
  plugins:[
    new uglifyjsWebpackPlugin()
  ]
})
```

### <b style="color:pink">_4.合并依赖文件_</b>

- 安装合并模块：`npm install webpack-merge --save-dev`

### <b style="color:pink">_5.配置相应脚本_</b>

- 将`webpack.config.js`文件分离之后，可以删除原先的配置文件了，但是注意此时`npm run serve / build`会报错，找不到默认的`webpck.config.js`文件，需要配置一下脚本，使用`--config`来指定配置文件
- `package.json`

```json
"dev": "webpack-dev-serve --open --config ./build/dev.config.js"
"build": "webpack --config ./build/prod.config.js"
```
