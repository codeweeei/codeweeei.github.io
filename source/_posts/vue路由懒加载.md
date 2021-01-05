---
title: vue路由懒加载
date: 2021/1/1
tags: [路由]
categories: Vue
---

## vue 路由懒加载

### 一：为什么要使用路由懒加载？

- 给客户更好的客户体验，首屏组件加载速度更快一些，解决白屏问题；

### 二：定义

- 懒加载简单来说就是延迟加载或按需加载，即在需要的时候进行加载；

### 三：未使用路由懒加载

```js
import Vue from "vue"
import VueRouter from "vue-router"
import Foo from "@/views/Foo.vue"

Vue.use(VueRouter)
const routes = [
  {
    path: "/foo",
    name: "Foo",
    component: Foo,
  },
]
const router = new VueRouter({
  routes,
  mode: "history",
})
```

### 四：路由懒加载的使用

- ES6 中的 import

```js
import Vue from "vue"
import VueRouter from "vue-router"
const Foo = () => import "@/views/Foo.vue"

Vue.use(VueRouter)
const routes = [
  {
    path: "/foo",
    name: "Foo",
    component: Foo,
  },
]
const router = new VueRouter({
  routes,
  mode: "history",
})
```

### 五：路由懒加载做到性能提升的原因

- webpack 会将懒加载的路由分块打包到一个单独的 js 中去，只有加载该路由的时候，才会加载这个 chunk 文件。
- 如果不使用路由懒加载，当首屏页面一加载时其他路由中的内容也会增加到 app.js 文件中，就会增大该文件的大小，从而降低首屏页面加载时间；

---

<span style="font-size:12px">
该篇文章引用自：

- [博客园](https://www.cnblogs.com/xiaoxiaoxun/p/11001884.html)
- [CSDN](https://blog.csdn.net/guxin_duyin/article/details/104720312)

</span>
