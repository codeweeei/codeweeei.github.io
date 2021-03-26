---
title: axios
date: 2021/3/26
tags: [ajax]
categories: Vue
---

## axios

> 实战时封装的 axios 的 baseURL 可能不同，此时就需要封装 axios 函数

### 简单封装 axios 函数

> 便于后期维护以及解耦（对引入的第三方库进行封装，不让多文件直接对第三方库进行直接依赖）

- network 文件夹 -> request.js 文件

```js
//封装axios函数
import axios from "axios"
export function request(config) {
  //1. 创建axios实例
  const instance = axios.create({
    baseURL: "",
    timeout: 5000,
  })
  //2.axios实例的拦截器（注意拦截之后需要将参数返回）
  //添加请求拦截器
  instance.interceptors.request.use(
    (config) => {
      // 在发送请求之前做些什么
      // 1.比如config中的一些信息不符合服务器的要求，配置headers
      // 2.展示加载时的图表
      // 3.某些网络请求（比如登录（token）），必须携带一些特殊的信息
      return config
    },
    (err) => {
      // 对请求错误做些什么
      return Promise.reject(err)
    }
  )
  //添加响应拦截器
  instance.interceptors.response.use(
    (res) => {
      // 对响应数据做点什么
      // 隐藏请求加载时的图表
      return res
    },
    function (err) {
      // 对响应错误做点什么
      return Promise.reject(err)
    }
  )
  //3.发送真正的网络请求，返回promise实例
  return instance(config)
}
```

- network -> home.js

```js
//关于home首页的网络请求放置该文件
import { request } from "./request.js"

export function getHomeMultidata() {
  return request({
    url: "/index",
  })
}
```

- views -> Home.vue
- 直接面向 home.js 获取数据

```js
import { getHomeMultidata } from "@/network/home.js"
const res = await getHomeMultidata()
```
