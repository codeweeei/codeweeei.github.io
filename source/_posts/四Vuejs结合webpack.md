---
title: 四：Vue结合Webpack
date: 2021/1/24
tags: [Vue知识全解]
categories: Vue
---

# Vue 知识全解

## <b style="color:skyblue">四：Vue 结合 Webpack 打包</b>

### <b style="color:pink">_1.引入 Vue.js_</b>

- 后续需要使用 vue.js 开发，就需要在 webpack 环境<span style="font-size:12px">[(点击跳转 webpack)](https://codeweeei.github.io/blog/webpack%E6%89%93%E5%8C%85%E5%B7%A5%E5%85%B7%E7%9A%84%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8/)</span>中集成 Vue.js；
- 通过 npm 安装 vue 模块在 node 包里，就可以在项目任何地方通过 import 引入 vue：
  - 命令行安装 vue：`npm install vue --save`（运行时依赖）
- `main.js`

```js
import Vue from "vue"
const app = new Vue({
  el: "#app",
})
```

### <b style="color:pink">_2. Vue.js 的版本_</b>

- 打包，会多生成一个 vue 的 js 文件，但是运行程序时可能会报 vue 版本的错；
- runtime-only 版本：代码中，不允许有任何的 template，出现则会报错；
- runtime-compiler 版本：代码中，允许有 template，因为 compiler 可以编译 template；
- 解决方案：在`webpack.config.js`里添加一个 resolve：

```js
resolve: {
  alias: {
    'vue$': 'vue/dist/vue.esm.js'
  }
}
```

### <b style="color:pink">_3. 创建 vue 时 template 和 el 的关系_</b>

- 后续使用 vue 做项目时一般都是做单页面富应用（项目中只有一个.html 文件），由于 el 挂载在.html 里，与#app 进行绑定，所以后续开发中，需要经常修改.html 文件里的内容，但是我们不希望常常修改我们的 html 文件，此时就可以在创建 Vue 实例时添加 template 属性；
- `main.js`

```js
new Vue({
  el: "#app",
  template: `
    <div>
      <h2>{{message}}</h2>
    </div>
  `,
  data: {
    message: "xxx",
  },
})
```

- `index.html`

```html
<div id="app"></div>
```

- 当同时有 el 和 template 时，vue 内部会将 template 模板里的内容会替换掉 el 挂载 的内容，就不需要在.html 文件里写大量代码了；

### <b style="color:pink">_4. main 的抽离方案_</b>

- 上述过程虽然可以做到后续开发不再需要再次操作 index.js，但是书写 template 也会变得非常麻烦，所以就需要将 template 模板进行抽离在一个 app.js 文件中；
- 借助组件化开发的思想，将 template 抽离成一个组件
- `app.js`

```js
const App = {
  template: `
    <div>
      <h2>{{message}}</h2>
      <button @click="btnClick">按钮<button>
    </div>
  `,
  data() {
    return {
      message: "xxx",
    }
  },
  methods: {
    btnClick() {
      console.log("点了")
    },
  },
}
export default App
```

- `main.js`

```js
import Vue from 'vue'
import App from './app.js'
const app = new Vue{
  el: "#app",
  template: "<App />",
  components: {
    App,
  }
}
```

### <b style="color:pink">_5. vue 的最终方案，封装.vue 文件_</b>

- 一个组件以一个 js 对象的形式进行组织和使用的时候是非常不方便的，一方面编写 template 模块非常的麻烦，另外一方面样式编写麻烦；
- 所以可以封装成一个.vue 文件：将 js 分成三部分：template，script，style；
- `App.vue`

```html
<template>
  <div>
    <h2 class="title">{{ message }}</h2>
    <button @click="btnClick">按钮</button>
  </div>
</template>

<script>
  export default {
    name: "App",
    data() {
      return {
        message: "xxx",
      }
    },
    methods: {
      btnClick() {
        console.log("点了")
      },
    },
  }
</script>

<style scoped>
  .title {
    color: blue;
  }
</style>
```

- `main.js`

```js
import Vue from 'vue'
import App from './App.vue'
const app = new Vue{
  el: "#app",
  template: "<App />",
  components: {
    App,
  }
}
```

- 但是此时直接用 webpack 打包会报错，因为没有配置 vue 相应的 loader
  - 安装需要的 loader
    - vue-loader：加载 vue 文件
    - vue-template-compiler：对 vue 进行编译
  - `npm install vue-loader vue-template-compiler --save-dev`开发时依赖
  - 配置 loader
  - `webpack.config.js`
    ```js
    rules: [
      {
        test: /\.vue$/,
        use: ["vue-loader"],
      },
    ]
    ```
- 注意事项： vue-loader 版本 14 之后的就需要添加插件，否则会报错，所以可以指定安装 13 版本的；
  - 可在 package.json 里将安装的 vue-loader 改为^13.0.0（意思就是版本大于 13，小于 14），然后重新`npm install`即可正常打包运行；
- 使用 import 引入模块时名称简写配置：正常引入 vue 模块需要`import vue from './vue.js'`，而经过配置之后就可以省略后缀文件`.vue`
- `webpack.config.js`

```js
const path = require("path")

module.exports = {
  resolve: {
    extensions: [".js", ".vue", ".css"],
  },
}
```
