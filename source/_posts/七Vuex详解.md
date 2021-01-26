---
title: 七：Vuex详解
date: 2021/1/27
tags: [Vue知识全解]
categories: Vue
---

# Vue 知识全解

## <span style="color:skyblue">七：Vuex 详解</span>

### <b style="color:pink">_1.Vuex 的介绍_</b>

- 官方解释：Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。它采用 集中式存储管理 应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。
- Vuex 也集成到 Vue 的官方调试工具  devtools extension（vue devtools），提供了诸如零配置的 time-travel 调试、状态快照导入导出等高级调试功能。
- 状态管理：多个组件共享的变量全部存储在一个对象中，将该对象放在顶层的 Vue 实例中，就可以让多个组件共享这个对象的所有变量属性了。
- 比如用户的登录状态、用户名称、头像、地理位置信息，商品的收藏、购物车中的物品等等。这些状态信息，我们都可以放在统一的地方，对它进行保存和管理，而且还是响应式的；

### <b style="color:pink">_2.Vuex 的安装与使用_</b>

- `npm install vuex --save`
- 最基本的 Vuex 的计数器使用
  - Vuex 应用的核心就是 store（仓库）`store`基本上就是一个容器，它包含着你的应用中大部分的状态 (state)。
  - 不能直接改变 store 中的状态。改变 store 中的状态的唯一途径就是显式地提交 (commit) mutation。这样使得我们可以方便地跟踪每一个状态的变化，从而让我们能够实现一些工具帮助我们更好地了解我们的应用。

```js
//store文件夹 - index.js
import Vue from "vue"
import Vuex from "vuex"
//安装Vuex
Vue.use(Vuex)
//创建store实例
const store = new Vuex({
  state: {
    count: 0,
  },
  mutation: {
    increment(state) {
      state.count++
    },
  },
})
export default store
```

- 将 store 注入到 vue 实例，即可在每个 Vue 组件通过 `this.$store` 来得到 store 实例
  - 通过 `this.$store.state.属性`的方式来访问状态
  - 通过 `this.$store.commit('mutation 中方法')`来修改状态

```js
//main.js
import Vue from "vue"
import store from "./store/index.js"
import App from "./components/App.vue"
const App = new Vue({
  el: "#app",
  store,
  render: (h) => h(App),
})
```

- 在 App 组件通过 commit 提交 mutation 里的方法来改变 state 里的状态

```html
<!-- App.vue -->
<template>
  <div id="app">
    <button @click="btnClick">+</button>
  </div>
</template>

<script>
  export default {
    name: App,
    methods: {
      btnClick() {
        this.$store.commit("increment")
      },
    },
  }
</script>
```

### <b style="color:pink">_3.Vuex 的核心概念 State_</b>

- State：Vuex 单一状态树（单一数据源），用一个对象就包含了全部的应用层级状态，每个应用将仅仅包含一个 store 实例。

### <b style="color:pink">_4.Vuex 的核心概念 Getters_</b>

- Getters：Vuex 允许我们在 store 中定义“getter”（可以认为是 store 的计算属性）。就像计算属性一样，getter 的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。
- Getters 接受第一个参数 state

```js
getters: {
  foo: (state) => {
    return state.count * state.count
  }
}
```

- Getter 也可以接受其他 getter 作为第二个参数：

```js
state: {
  players: [
    {
      name: kobe,
      length: 198,
    },
    {
      name: jeams,
      length: 203,
    },
    {
      name: westbrook,
      length: 191,
    },
  ]
}
getters: {
  more195: (state) => {
    return state.players.filter((player) => player.length > 195)
  }
  more195length: (state, getters) => {
    return getters.more195.length //2
  }
}
```

### <b style="color:pink">_5.Vuex 的核心概念 Mutation_</b>

- Mutation：更改 Vuex 的 store 中的状态的唯一方法是提交 mutation，每个 mutation 都有一个字符串的 事件类型 (type) 和 一个 回调函数 (handler)，该回调函数就是实际进行状态更改的地方，并且接受 state 作为第一个参数

```js
new store = Vuex({
  state: {
    count: 0
  }
  mutaton: {
    //状态更改
    increment(state){
      state.count++
    }
  }
})

```

```html
<!-- 通过commit调用 -->
<button @click="$store.commit('increment')">+1</button>
```

- 也可以向 store.commit 传入额外的参数，即 mutation 的 载荷（payload）
- 基本传法（传入一个参数）：

```js
new store = Vuex({
  state: {
    count: 0
  }
  mutatons: {
    //状态更改
    increment(state,n){
      state.count += n
    }
  }
})
```

```html
<!-- 通过commit调用 -->
<button @click="$store.commit('increment',5)">+5</button>
```

- 传入一个对象：

```js
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}
```

```js
//调用
store.commit("increment", {
  amount: 10,
})
```

- 对象风格的提交方式

  - 提交 mutation 的另一种方式是直接使用包含 type 属性的对象：

  ```js
  store.commit({
    type: "increment",
    amount: 10,
  })

  mutations: {
    increment (state, payload) {
      state.count += payload.amount
    }
  }
  ```

- mutation 的响应规则

  - Vuex 的 store 中的 state 是响应式的, 当 state 中的数据发生改变时, Vue 组件会自动更新.
  - 需要遵循一些规则：
    - 提前在 store 中初始化好所需的属性，这些属性都会被加入到响应式系统，响应式系统会监听这些属性的变化，当属性发生变化时，会通知所有界面中用到该属性的地方，让界面发生刷新。
    - 当给 state 中的对象添加新属性时, 使用下面的方式:
      - 方式一: 使用 `Vue.set(obj, 'newProp', 123)`
      - 方式二: 用新对象给旧对象重新赋值
    - 使用`Vue.delete(obj,'prop')`用来删除已有的属性

- 使用常量替代 Mutation 事件的类型

  - 创建一个文件: mutation-types.js, 并且在其中定义我们的常量.
  - 定义常量时, 我们可以使用 ES2015 中的风格, 使用一个常量来作为函数的名称.

```js
//store - mutations-types.js
export const INCREMENT = "increment"

//store - index.js
import { INCREMENT } from "./mutations-types"

mutations: {
  [INCREMENT](state){
    state.count++
  }
}
```

> <b style="color:#f55">mutation 必须是同步函数</b>

### <b style="color:pink">_6.Vuex 的核心概念 Action_</b>

- Action：类似于 mutation，但是可以替代 mutation 来进行异步操作

```js
//store - index.js
mutations: {
  increment(state){
    state.count++
  }
},
actions: {
  updataInfo(context){
    //context上下文（store对象），再通过commit调用mutations里的方法
    context.commit('increment')
  }
}
```

- 在 vue 组件时通过 dispatch 分发 Action

  - Action 通过 `store.dispatch` 方法触发：

  ```js
  store.dispatch("updataInfo")
  ```

- Actions 支持同样的载荷方式和对象方式进行分发：

```js
// 以载荷形式分发
store.dispatch("updataInfo", {
  amount: 10,
})

// 以对象形式分发
store.dispatch({
  type: "updataInfo",
  amount: 10,
})
```

### <b style="color:pink">_7.Vuex 的核心概念 Module_</b>

- Module：Vue 使用单一状态树,那么也意味着很多状态都会交给 Vuex 来管理.当应用变得非常复杂时,store 对象就有可能变得相当臃肿.为了解决这个问题, Vuex 允许我们将 store 分割成模块(Module), 而每个模块拥有自己的 state、mutation、action、getters 等

```js
const moduleA = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

store.state.a // -> 获取moduleA 的状态
store.state.b // -> 获取moduleB 的状态
```

- 对于模块内部的 mutation 和 getter，接收的第一个参数是模块的局部状态对象。
- rootState 为根节点状态

### <b style="color:pink">_8.Vuex 的项目结构_</b>

- 图片示例
  {% asset_img vuex项目结构.png vuex项目结构%}
