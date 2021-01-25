---
title: 六：VueRouter路由详解
date: 2021/1/26
tags: [Vue知识全解]
categories: Vue
---

# Vue 知识全解

## <span style="color:skyblue">六：Vue-Router 详解</span>

### <b style="color:pink">_1.路由的介绍_</b>

- 路由就是通过互联网的网络把信息从源地址传输到目的地址的活动；
- 路由提供的两种机制：
  - 路由：路由是决定数据包从来源到目的地的路径
  - 转送：转送将输入端的数据转移到合适的输出端

### <b style="color:pink">_2.后端渲染和前端渲染阶段_</b>

- 后端渲染（早期）阶段：

  > 后端路由：后端处理 URL 和页面之间的映射关系。

  - 早期的网站开发整个 HTML 页面是由服务器来渲染的,服务器直接生产渲染好对应的 HTML 页面, 返回给客户端进行展示，一个页面有自己对应的网址, 也就是 URL，URL 会发送到服务器, 服务器会通过正则对该 URL 进行匹配, 并且最后交给一个 Controller 进行处理，Controller 进行各种处理, 最终生成 HTML 或者数据, 返回给前端，这就完成了一个 IO 操作；
  - 当页面中需要请求不同的路径内容时, 交给服务器来进行处理, 服务器渲染好整个页面, 并且将页面返回给客户顿.这种情况下渲染好的页面, 不需要单独加载任何的 js 和 css, 可以直接交给浏览器展示, 这样也有利于 SEO 的优化.
  - 后端路由的弊端
    - 整个页面模块都是由后端人员来编写和维护的
    - 前端人员要开发页面，必须通过 PHP 和 Java 等语言来编写代码
    - html 代码和数据以及相应的逻辑会混在一起,编写和维护比较麻烦

- 前后端分离阶段

  > 随着 Ajax 的出现,有了前后端分离阶段

  - 后端只负责提供数据,不负责任何阶段的内容
  - 后端只提供 API 来返回数据, 前端通过 Ajax 获取数据, 并且可以通过 JavaScript 将数据渲染到页面中.
  - 一个 url 对应一套 html+css+js
  - 优点: 前后端责任的清晰, 后端专注于数据上, 前端专注于交互和可视化上.

- 单页面富应用 SPA 页面(前端渲染)阶段
  > url 和页面(组件)的映射关系由前端管理(vue-router)
  - 整个网址只有一个 html 页面,改变 URL，但是页面不进行整体的刷新
  - 只有一整套的 html+css+js 资源,当页面 url 进行变化时,会从资源中抽离出该 url 对应的资源

### <b style="color:pink">_3.url 的 hash 和 html5 的 history_</b>

- 如何做到改变 url 而页面不刷新
  1. (使用 hash 模式(默认))url 的 hash 就是 url 的锚点,本质上就是改变 `window.location` 的 `hash` 属性进而改变 url,但页面不会进行刷新;
  ```js
  //原url:http://192.168.1.101:8000/examples
  window.location.hash = "foo"
  //改完之后的url:http://192.168.1.101:8000/examples/foo
  ```
  2. (使用 h5 的 history 模式)`history.pushState({},'','foo')`,
     相当于压栈和出栈,永远显示栈顶的路径,所以存在历史记录;
     - `history.back()`可以退回上次的历史记录 => `history.go(-1)`
     - `history.forward()`可以向前一次的记录 => `history.go(1)`
  3. `history.replaceState()`跟上述很相似,只是没有历史记录;

### <b style="color:pink">_4.vue-router 的安装与配置_</b>

- vue-router 是 vue.js 官方的路由插件,用于构建单页面应用;
- vue-router 是基于路由和组件的
  - 路由用于设定访问路径,将路径和组件映射起来
  - 在 vue-router 的单页面应用中,页面的路径的改变就是组件的切换
- <b>安装 vue-router</b>
  - `npm install vue-router --save`
- <b>使用路由</b>

  1. 导入路由对象,并且调用 Vue.use(VueRouter)来安装路由功能
  2. 创建路由实例,并且传入路由映射关系

  ```js
  //router文件夹里创建index.js
  import Vue from "vue"
  import VueRouter from "vue-router"
  //安装VueRouter插件
  Vue.use(VueRouter)
  //配置路径和组件之间的映射关系
  const routes = []
  const router = new VueRouter({
    routes,
  })
  //导出router实例
  export default router
  ```

  3. 在 Vue 实例挂载创建的路由实例

  - `main.js`

  ```js
  import router from "./router/index"
  new Vue({
    el: "#app",
    //vue实例上挂载创建的路由实例
    router,
    //路径模式改为history模式
    mode: 'history'
    render: (h) => h(App),
  })
  ```

### <b style="color:pink">_5.配置 vue-router 的路由映射关系并呈现_</b>

1. 创建路由组件:在 components 里创建`Home.vue和About.vue`文件
2. 配置映射关系

```js
//router - index.js
import Home from "@/components/Home"
import About from "@/components/About"
const routes = [
  //-设置路由的默认值(重定向)
  {
    path: "/",
    //redirect可以把根路径/重定向为/home的路径
    redirect: "/home",
  },
  {
    path: "/home",
    component: Home,
  },
  {
    path: "/about",
    component: About,
  },
]
```

3. 呈现路由

- `router-link` 标签是一个 vue-router 中已经内置的组件, 它会被渲染成一个`a`标签,点击就会切换改变路径;
- `router-view` 标签会根据当前的路径, 动态渲染出不同的组件.
- 在路由切换时, 切换的是`router-view`挂载的组件, 其他内容不会发生改变.
- `App.vue`

```html
<template>
  <div id="app">
    <router-link to="/home">首页</router-link>
    <router-link to="/about">关于</router-link>
    <router-view />
  </div>
</template>
```

4. 修改路径为 history 模式

- 默认是路径 hash 值的模式,就是路径前会跟上#,这样路径不美观
- 可以在创建 router 实例时,将`mode`(options)改为`'history'`

### <b style="color:pink">_6. router-link 的其他属性_</b>

- to 属性:用于跳转指定的路径,默认渲染成`a`标签
- tag 属性:用于指定`router-link`之后渲染成什么标签
  - `<router-link to='/home' tag='li'>`就会渲染成 li 元素
- replace 属性: replace 不会留下 history 记录, 所以指定 replace 的情况下, 后退键返回不能返回到上一个页面中(默认是 push)
  - `<router-link to="home" replace></router-link>`
- active-class 属性: 当`router-link`对应的路由匹配成功时, 会自动给当前元素设置一个 `router-link-active` 的 class, 设置 active-class 可以修改默认的名称,或者创建 router 实例时设置 linkActiveClass 的 options 可以统一修改默认的名称.
  - `<router-link to="home" active-class="newactive">首页</router-link>`
  ```js
  //router - index.js
  const router = new VueRouter({
    routes,
    mode: "history",
    linkActiveClass: "newactive",
  })
  ```

### <b style="color:pink">_7. 通过代码跳转路由_</b>

- 增加点击事件监听方法:
  - `this.$router.push('/home')`即可跳转到/home 路径(有历史记录)
  - `this.$router.replace('/home')`也可跳转到/home 路径(无历史记录)

### <b style="color:pink">_8. 动态路由_</b>

- 在某些情况下，一个页面的 path 路径是不确定的：例如 `/user:zhangsan`

```js
//router - index.js
import User from "@/components/User"
const routes = [
  {
    path: "/user/:userId",
    component: "User",
  },
]
```

```html
<!-- 2.组件跳转时直接加上/id -->
<router-link to="/User/zhangsan">用户</router-link>
```

- 此时点击用户就会跳转到`/user/zhangsan`的路径
- 真实开发环境是需要获取到 id 变量并保存，然后使用 v-bind 属性动态绑定决定跳转的路径；
- 使用 `this.$route.params.id`来获取处于活跃的路由路径的 `userId`

### <b style="color:pink">_9. 路由的懒加载_</b>

- 当打包构建应用时，JavaScript 包会变得很大，影响页面加载；
  - 即路由中会定义很多不同的页面，这些页面最后会被打包在同一个 js 文件中，必然会造成该 js 文件比较大
  - 所以如果一次性从服务器请求下来这个页面, 可能需要花费一定的时间, 甚至用户的电脑上还会出现短暂空白的情况.
- 解决方法：使用路由懒加载
  - 路由懒加载的主要作用就是将路由对应的组件打包成在一个个的 对应的 小 js 代码块.
  - 只有在这个路由被访问到的时候, 才加载对应的组件（引入对应的 js 代码）
- 路由懒加载的使用：

```js
//方式一
const routes = [
  {
    path: "/home",
    component: () => import("@/components/Home"),
  },
  {
    path: "/about",
    component: () => import("@/components/About"),
  },
]
```

```js
//方式二(推荐使用)
const Home = () => import("@/components/Home")
const About = () => import("@/components/About")
const routes = [
  {
    path: "/home",
    component: Home,
  },
  {
    path: "/about",
    component: About,
  },
]
```

### <b style="color:pink">_10. 路由的嵌套_</b>

- 路由嵌套是一个很常见的功能：比如在 home 页面中，可以通过`/home/news` 访问消息，一个路由映射一个组件，所以 news 需要创建一个新的组件。
- 步骤
  - 创建对应的子组件，并且在路由映射中配置对应的子路由；
  - 在父组件内部使用`<router-view></router-view>`标签将其显示出来；
- 代码样式：

```js
//router - index.js
const Home = () => import("@/components/Home")
const News = () => import("@/components/News")
const routes = [
  {
    path: "/home",
    component: Home,
    children: [
      {
        path: "",
        //重定向默认路径
        redirect: "news"
      }
      {
        path: "news", //这里路径注意不要加/，会自动拼接在home后面
        component: News,
      },
    ],
  },
]
```

```html
<!-- Home -->
<template>
  <div>
    <h2>首页</h2>
    <router-link to="/home/news">消息</router-link>
    <router-view></router-view>
  </div>
</template>
```

### <b style="color:pink">_11.路由跳转传递参数_</b>

- router-link 方式

  - params 参数传递（类似动态路由）
    - 配置路由格式：/router/:id
    - 传递的方式：在 path 后面跟上对应的值（xxx）
    - 传递后形成的路径：/router/xxx
    - 使用 `this.$route.params.id` 来得到 xxx 的值
  - query 参数传递（可传递对象）

    - 配置路由格式：使用对象的形式来传递，普通配置
    - 传递后形成的路径：`/profile?name=cw&age=22`
    - 使用`this.$route.query.name`来得到 cw

    ```html
    <!-- query参数传递 -->
    <router-link :to="{path: '/profile', query: {name: 'cw', age: '22'}}"
      >档案</router-link
    >
    ```

- JavaScript 方式传递参数
  - params
  ```js
  this.$router.push("/profile" + this.userId)
  ```
  - query
  ```js
  this.$router.push({
    path: "/profile",
    query: {
      name: "cw",
      age: 22,
    },
  })
  ```

### <b style="color:pink">_12.$router和$route 的区别_</b>

- $route和$router 的区别
  - $router 为 VueRouter 实例，想要导航到不同 URL，可使用 push、replace、go、back、forward 等方法
  - $route 为当前 router 跳转对象（活跃路由），里面可以获取 name、path、query、params、meta 等属性

### <b style="color:pink">_13.导航守卫_</b>

- vue-router 提供的导航守卫主要用来监听监听路由的进入和离开的.
- vue-router 提供了 beforeEach 和 afterEach 的钩子函数, 它们会在路由即将改变前和改变后触发.
- 可以使用 全局前置守卫 beforeEach 函数

  ```js
  router.beforeEach((to, from, next) => {
    //to：表示即将要跳转的目标路由对象
    //from：表示导航即将要离开的路由对象
    //next：调用该方法后，才能进入下一个钩子，而且可以直接跳转到相应路径next(/login)
  })
  ```

- 例子：在每一次跳转路由时更改网页的 title
  ```js
  //router - index.js
  const route = [
    {
      path: "/home",
      component: Home,
      //使用meta元数据定义路由的标题
      meta: {
        title: "首页",
      },
    },
    {
      path: "/about",
      component: About,
      meta: {
        title: "关于",
      },
    },
  ]
  router.beforeEach((to, from, next) => {
    //将meta的title传给网页标题，注意当有路由嵌套时需要matched[0]才能拿到匹配的meta
    window.document.title = to.matched[0].meta.title
    next()
  })
  ```
- 补充：如果是后置钩子(跳转完之后执行), 也就是 调用 afterEach((to,from) => {}), 不需要主动调用 next()函数

---

- <b style="color:skyblue">路由独享守卫</b>
  - 可以在路由配置中直接定义`beforeEnter`守卫

```js
const router = new VueRouter({
  routes: [
    {
      path: "/foo",
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...与之前前置守卫方法参数一致
      },
    },
  ],
})
```

- <b style="color:skyblue">组件内的守卫</b>
  - [详情可查看官网](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#%E8%B7%AF%E7%94%B1%E7%8B%AC%E4%BA%AB%E7%9A%84%E5%AE%88%E5%8D%AB)

### <b style="color:pink">_14.keep-alive 的使用_</b>

- keep-alive 是 Vue 内置的一个组件，可以使被包含的组件保留状态，或避免重新渲染。
- 两个重要属性:
  - include - 字符串或正则表达（不要随意加空格），只有匹配的组件会被缓存
  - exclude - 字符串或正则表达式，任何匹配的组件都不会被缓存
- router-view 也是一个组件，如果直接被包在 keep-alive 里面，所有路径匹配到的视图组件都会被缓存：

```html
<router-alive exclude="组件name1,...">
  <router-view>
    <!-- 这里的除了组件name1的组件，其他视图组件都会被缓存 -->
  </router-view>
</router-alive>
```
