---
title: vue兄弟组件之间的通信
date: 2020/12/13
tags: vue组件
categories: Vue
---


### 兄弟组件之间的通信

1. 子 1 $emit() —>父 props —>子 2
2. 通过事件总线来传送
   1. 在 main.js 里将 EventBus=new Vue(),将其导出，然后在子 1 导入，通过 EventBus.$emit()发出信息，在子2也导入，通过EventBus.$on()来接收信息；
   2. 在 main.js 里将$EventBus挂载在Vue.prototype里，即可在任意组件里使用this.$EventBus 来进行通信；
3. 通过 Vuex 来进行兄弟组件之间的通信

   ```js
   //1.首先在根目录里创建store文件夹，里面新建index.js文件，创建store实例，然后将其导出

   //index.js
   import Vue from "vue"
   import Vuex from "vuex"

   Vue.use(Vuex)

   const store = new Vuex.Store({
     state: {
       //保存状态（变量）
       name: "xxx",
     },
     mutations: {
       //mutations里可以用来修改state里的值，默认会传入state参数
       ChangeName(state) {
         state.name = "yyy"
       },
     },
   })

   export default store

   //2.在main.js导入store，将其注入到根Vue实例里，挂载后任何组件内都可以使用$store.state来访问store里面的内容；

   //3.在需要共享的组件里使用$store.state即可进行访问state里的数据，组件内的内容需要通过this.$store.commit('mutations方法的名称')来访问mutations里的方法对应内容；
   ```
