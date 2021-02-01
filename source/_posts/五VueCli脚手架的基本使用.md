---
title: Vue入门篇五：Vue-Cli脚手架的基本使用
date: 2021/1/25
tags: [Vue知识全解]
categories: Vue
---

# Vue.js 知识全解

## <span style="color:skyblue">五：Vue-cli 脚手架详解</span>

### <b style="color:pink">_1.vue-cli 脚手架的介绍_</b>

- 当在开发大型项目时，就必然需要 Vue Cli（command-line interface）命令行界面，俗称脚手架；
- 使用 vue-cli 可以快速搭建 vue 开发环境以及对应的 webpack 配置；
- 使用前提：依赖于 Node 环境，使用了 webpack；

### <b style="color:pink">_2.vue-cli 脚手架的使用_</b>

- 全局安装 vue-cli 脚手架：
  `npm install -g @vue/cli`
- 使用脚手架搭建项目：
  - Vue CLI >= 3 :`vue create 项目名称`
  - Vue CLI < 3 :`vue init webpack 项目名称`
- Vue CLI >= 3 和旧版使用了相同的 vue 命令，所以 Vue CLI 2 (vue-cli) 被覆盖了。如果你仍然需要使用旧版本的 vue init 功能，你可以全局安装一个桥接工具：

  `npm install -g @vue/cli-init`
  `vue init` 的运行效果将会跟 `vue-cli@2.x` 相同

---

### <b style="color:pink">_3.vue-cli2 脚手架创建项目_</b>

- 创建过程（如下图）
  {% asset_img vuecli2创建项目过程.png vuecli2创建项目 %}
- 创建之后的目录结构（如下图）
  {% asset_img vuecli2项目结构.png vuecli2项目结构 %}

---

### <b style="color:pink">_4.runtime-compiler 与 runtime-only 的区别_</b>

- 两者的区别在于 main.js 文件
- `runtime-compiler`

```js
//main.js
import Vue from "vue"
import App from "./App.vue"

Vue.config.productionTip = false

new Vue({
  el: "#app",
  template: "<APP/>",
  components: { App },
})
```

- - vue 程序运行过程：template => ast（抽象语法树） =>compiler 成 对应的 render 函数 => virtual dom => ui

- `runtime-only`

```js
import Vue from "vue"
import App from "./App.vue"

Vue.config.productionTip = false

new Vue({
  el: "#app",
  render: (h) => h(App),
})
```

- render 函数：h 是 createElement 函数的代号；可传三个参数：`createElement（'标签','标签的属性',['标签的内容']）`，该函数同样可以创建一个标签（或者传入组件）替换掉 el 挂载的区域。
- - vue 程序运行过程： render 函数 => virtual dom => ui
  - 性能更高：.vue 文件里的 template 会被内部 vue（vue-template-compiler）解析成 render 函数，省去了 template 编译成 render 函数的步骤（推荐使用）
  - 更轻量

---

### <b style="color:pink">_5.vue-cli3 脚手架创建项目_</b>

- vue-cli 3 与 2 版本有很大区别

  - vue-cli 3 是基于 webpack 4 打造， vue-cli 2 还是 webapck 3
  - vue-cli 3 的设计原则是“0 配置”，根目录下移除了配置文件的，build 和 config 等目录
  - vue-cli 3 提供了 vue ui 命令，提供了可视化配置，更加人性化
  - 移除了 static 文件夹，新增了 public 文件夹，并且 index.html 移动到 public 中

- 创建之后的目录结构（如下图）
  {% asset_img vue-cli3脚手架项目目录结构.png vuecli3创建项目 %}

- `main.js`

```js
import Vue from "vue"
import App from "./App.vue"

Vue.config.productionTip = false

new Vue({
  render: (h) => h(App),
}).$mount("#app")
```

- el 挂载在内部相当于$mount("#app")

### <b style="color:pink">_6.vue-cli3 配置文件的查看和修改_</b>

- 使用`vue ui`命令行打开可视化用户界面（本地服务）
  - 导入创建的 vue 项目，进入项目仪表盘；
  - 可以查看已下载的插件和依赖并且下载新的依赖和插件；
- 打开 node_modules 包 -> @vue -> cli-serve -> webpack.config.js 即可查看相应的配置文件

> 修改 webpack 配置

- 在根目录创建 `vue.config.js`文件

```js
module.exports = {
  //新增的配置文件会合并到原来的配置文件中
}
```
