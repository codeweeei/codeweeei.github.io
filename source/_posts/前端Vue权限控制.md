---
title: 前端Vue权限控制
date: 2021/4/15
tags: [vue实战]
categories: Vue
---

# Vue 前端权限控制

## 权限相关概念

- 后端权限：后端权限可以控制某个用户是否能够查询数据，是否能够修改数据等操作；
  - 后端如何知道该请求是哪个用户发来的（状态保持）
  - cookie、session、token
- 前端权限：控制前端的视图层的展示和前端所发送的请求，但是只有前端权限控制没有后端权限是万万不可的

### 前端权限的意义

1. 降低非法操作的可能性
2. 尽可能排除不必要请求，减轻服务器的压力
3. 提高用户体验

### 前端权限控制思路

1. 菜单的控制

- 登录请求中，根据后端返回的权限数据，动态展示对应的菜单
- 后台管理的侧边栏（没有权限需要隐藏一些菜单）

2. 界面的控制

- 如果用户没有登陆，手动在地址栏上敲入管理界面的地址，则需要跳转到登录界面
- 如果用户已经登录，可是手动敲入非权限内的地址，则需要跳转到 404 界面

3. 按钮的控制

- 在某个菜单的界面中，还得根据权限数据，展示出可进行操作的按钮，比如删除，修改，增加

4. 请求和响应的控制

- 如果用户通过非常规操作，比如通过浏览器调试工具将某些禁用的按钮变成启动状态，此时发的请求，也应该被前端所拦截

## Vue 的权限控制实现

### 权限菜单栏控制实现

- 用户登录之后服务端返回一个数据，这个数据有菜单列表和 `token`，我们把这个数据放入到 `vuex` 中，然后主页根据 `vuex` 中的数据进行菜单列表的渲染
- 问题：刷新界面`vuex`数据消失，菜单栏消失
- 解决：将数据存储在`sessionStroage`中，让其和`vuex`中的数据保持同步

### 权限界面的控制实现

- 用户登录成功后，将 token 数据存储在`sessionStroage`中，判断是否登录
- 路由导航守卫

```js
router.beforeEach((to,from,next) ={
  if(to.path === '/login'){
    next()
  }else{
    const token = sessionStroage.getItem('token')
    if(!token){
      next('/login')
    }else{
      next()
    }
  }
})
```

- 问题：这样<span style="color:skyblue;">用户在登录之后就可以访问其他界面了</span>，但如果用户 A 登录之后他只能访问 a 页面，他不能访问 b 页面，但是这时候他还是可以通过地址栏输入进入到 b 页面;
- 解决方法：使用`动态路由`来对 A 用户不显示 b 页面

### Vue 动态路由实现权限界面的控制

- 根据当前用户所拥有的权限数据来动态添加所需要的路由

1. 先定义好所有的路由规则

```js
const userRule = { path: "/users", component: Users }
const roleRule = { path: "/roles", component: Roles }
const goodRule = { path: "/goods", component: GoodsList }
const categoryRule = { path: "/categories", component: GoodsCate }
```

2. 登录成功之后动态添加路由，注意这个`initDynamicRoutes`的方法需要暴露出去在登录页面调用

### 按钮权限控制

- 虽然用户可以看到某些界面了， 但是这个界面的一些按钮该用户可能是没有权限的。 因此， 我们需要对组件中的一些按钮进行控制， 用户不具备权限的按钮就隐藏或者禁用， 而在这块的实现中， 可以把该逻辑放到<span style="color:skyblue;">自定义指令</span>中

1. 首先根据后端传来的数据`right`属性来判断用户有什么权限，例如

```js
{
  //...
  right: ["view"]
}
{
  //...
  right: ["view", "add"]
}
```

2. 添加自定义指令控制按钮

```html
<button v-permisson="{action:'add',effect:'disabled'}"></button>
```

- permisson.js

```js
Vue.directive("permisson", {
  inserted: function (el, binding) {
    const action = binding.value.action //对应自定义指令中的add
    const currentRight = router.currentRoute.meta //获取当前路由元信息（当前路由的权限信息）
    if (currentRight) {
      if (currentRight.indexOf(action) == -1) {
        //不具备权限
        const type = binding.value.effect
        if (type === "disabled") {
          el.disabled = true //禁用按钮
          el.classList.add("is-disabled")
        } else {
          el.parentNode.removeChild(el) //移除按钮
        }
      }
    }
  },
})
```

### 请求和相应的控制

- 请求控制：除了登录请求都得要带上 token ， 这样服务器才可以鉴别你的身份
- axios 的请求拦截设置

```js
axios.interceptors.request.use(function (req) {
  const currentUrl = req.url
  if (currentUrl != "/login") {
    req.headers.Authorization = sessionStorage.getItem("token")
  }
  return req
})
```

- 如果发出了非权限内的请求， 应该直接在前端范围内阻止， 虽然这个请求发到服务器也会被拒绝
- 非权限内的请求：比如 a 用户是不能够操作该页面的按钮的，但是他通过 f12 调试把按钮改为可点击，如果我们不对这个请求进行处理，那么这个请求就会发送出去

```js
//请求方式的约定restful
// restful风格请求
const actionMapping = {
  get: "view",
  post: "add",
  put: "edit",
  delete: "delete",
}
axios.interceptors.request.use(function (req) {
  const currentUrl = req.url
  if (currentUrl != "/login") {
    req.headers.Authorization = sessionStorage.getItem("token")
    //当前模块具有的权限
    //查看 get请求
    //增加 post请求
    //修改 put请求
    //删除 delete请求
    const method = req.method //根据请求，获取哪种操作
    const action = actionMapping[method]
    //判断action是否存在当前路由的权限中
    const rights = router.currentRoute.meta
    if (rights.indexOf(action) == -1) {
      //没有权限
      return Promise.reject(new Error("没有权限"))
    }
  }
})
```

### 响应控制

- 当请求响应得到了服务器返回的状态码 401, 即代表 token 超时或者被篡改了，此时应该强制跳转到登录界面

```js
axios.interceptors.response.use(function (res) {
  if (res.data.meta.status === 401) {
    router.push("/login")
    sessionStorage.clear()
    window.location.reload()
  }
  return res
})
```
