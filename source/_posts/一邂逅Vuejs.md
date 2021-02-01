---
title: Vue入门篇一：邂逅Vue.js
date: 2021/1/13
tags: [Vue知识全解]
categories: Vue
---

# Vue.js 知识全解

## <span style="color:skyblue">一：邂逅 Vue.js</span>

### _1. Vue.js 是一个构建用户界面的渐进式的框架_

- 渐进表示可以将 Vue 作为应用的一部分嵌入其中，带来更多丰富的交互体验；
- 或者希望更多的业务逻辑使用 Vue 实现，一点一点将以前的页面进行重构；

### _2. Vue 的特点_

- 解耦视图和数据
- 可复用的组件
- 前端路由技术
- 状态管理
- 虚拟 DOM

### _3. Vue.js 的安装_

- 直接 CDN 引入

```html
<!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdeliver.net/npm/vue/dist/vue.js"></script>
<!-- 生产环境版本，优化了尺寸和速度 -->
<script src="https://cdn.jsdeliver.net/npm/vue"></script>
```

- 下载和引入

  - 开发环境：https://vuejs.org/js/vue.js
  - 生产环境：https://vuejs.org/js/vue.min.js

- <b style="color:pink">npm 安装</b>

### _4. Vue.js 的初体验_

- 传统编程范式：命令式编程
- Vue 的编程范式：声明式编程

```html
<div id="app">{{message}}</div>
<script src="js/vue.js"></script>
<script>
  //创建Vue实例对象
  const app = new Vue({
    el: "#app", //用来挂载Vue实例要管理的元素，就会解析该元素下的特殊语法与vue指令
    data: {
      //定义储存一些数据，可以直接定义出来，也可以来自网络，从服务器加载的
      messages: "xxx",
    },
  })
</script>
```

### _5. Vue 中的 MVVM_

- MVVM 是 Model-View-ViewModel 的缩写
- View 层表示视图层（DOM 层）——即给用户展示各种信息
- Model 层表示数据层 （可以是定义的数据，服务器请求的数据，网络上获取的数据等）
- ViewModel 表示视图模型层，是 View 和 Model 沟通的桥梁；即 Vue 实例
  - 一方面可以实现 Data Binding（数据绑定），即 Model 的改变实时的反映到 View 层；
  - 另一方面可以实现 DOM Listeners（DOM 监听），当 DOM 中发生一些事件（点击，滚动等），可以监听到，并在需要的时候更改对应的 Data；
- 图解{% asset_img mvvm.png mvvm %}

### _6. Vue 传入的 options 选项_

- el
  - 类型：string | HTMLElement
  - 作用：决定之后 Vue 挂载在哪个元素（管理哪个 DOM）
- data
  - 类型：Object | <b style="color:pink">Function（组件中必需是函数）</b>
  - 作用：Vue 实例对应的数据对象
- methods
  - 作用：定义属于 Vue 的一些方法，可以在其他地方调用，也可以在指令中使用

### _7. Vue 的八个生命周期_

- beforeCreate
  - 在这个钩子函数里，只是刚开始初始化实例，你拿不到实例里的任何东西，比如 data 和 methods 和事件监听等。
- <b style="color:skyblue">create</b>
  - 在实例创建完成后被立即调用。在这一步，实例已完成以下的配置：数据观测 (data observer)，属性和方法的运算，watch/event 事件回调。然而，挂载阶段还没开始，$el 属性目前不可见。
  - <b style="color:pink">应用场景：异步数据的获取和对实例数据的初始化操作都在这里面进行。</b>

```js
//常见使用
data: {
  msg: null
},
methods: {
  getMsg(){
    this.$http.get(url).then(res => {
      this.msg = res.data.msg
    })
  }
},
created() {
  this.getMsg()
}
```

- beforeMounted

  - 在挂载开始之前被调用：相关的 render 函数首次被调用。
  - 不论是 created 还是 beforeMount 在它们里面都拿不到真实的 dom 元素，如果我们需要拿到 dom 元素就需要在 mounted 里操作

- <b style="color:skyblue">mounted</b>
  - el 已经挂载对应的 dom 元素，即可以拿到初始化的 dom 元素，如果存在异步的操作对 dom 元素数据进行修改则只能在 updated 里获取；
  - <b style="color:pink">应用场景：初始数据（在 data 中有的）的 dom 渲染完毕，可以获取 dom</b>
- beforeUpdated
  - 当数据更新后出发的钩子函数，这个钩子函数里拿到的是更改之前的数据，虚拟 DOM 重新渲染和打补丁之前被调用。
- updated

  - updated 是指 mouted 钩子后（包括 mounted）的数据更改时调用，改变一次调用一次，在 created 里的数据更改不叫更改叫做初始化；
  - 一般都是在事件方法（methods）里更改数据，然后通过 updated 对其操作。

  - 应用场景：如果 dom 操作依赖的数据是在异步操作中获取，并且只有一次数据的更改 ，也可以说是数据更新完毕：如果对数据更新做一些统一处理在 updated 钩子中处理即可。
  - 如果想分别区别不同的数据更新，同时要对 dom 进行操作那么需要使用 nextTick 函数处理；
  - 注意事项：不要在当前钩子里修改当前组件中的 data，否则会继续触发 beforeUpdate、updated 这两个生命周期，进入死循环！
  - <b style="color:pink">updated，watch 和 nextTick 区别</b>
    - updated 对所有数据的变化进行统一处理
    - watch 对具体某个数据变化做统一处理
    - nextTick 是对某个数据的某一次变化进行处理

- beforeDestory
  - 实例销毁之前调用。在这一步，实例仍然完全可用
- destory

  - Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。
  - beforeDestroy 和 destroyed 只能通过手动触发$destroy 来调用

  ```js
  //手动触发销毁app Vue实例
  app.$destroy()
  ```

- 图解{% asset_img vue生命周期.png vue生命周期 %}
