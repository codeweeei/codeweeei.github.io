---
title: Ajax的使用
date: 2021/2/1
tags: [ajax]
categories: Javascript
---

# Ajax 的使用

## 1.什么是 Ajax

AJAX 是异步的 JavaScript 和 XML（Asynchronous JavaScript And XML）。简单点说，就是使用 XMLHttpRequest 对象与服务器通信。 它可以使用 JSON，XML，HTML 和 text 文本等格式发送和接收数据。AJAX 最吸引人的就是它的“异步”特性，也就是说它可以在不重新刷新页面的情况下与服务器通信，交换数据，或更新页面。

你可以使用 AJAX 最主要的两个特性做下列事：

- 在不重新加载页面的情况下发送请求给服务器
- 接受并使用从服务器发来的数据
- 可以使用 get 和 post 等 type 方式请求
- 请求时不会刷新页面
- 与服务器交互比较灵活

## 2.Ajax 的基本使用

- 4 步操作
  - 1、new 一个 xhr 对象
  - 2、调用 xhr 对象的 open 方法
  - 3、send 一些数据（字符串）
  - 4、对服务器的响应过程进行监听，来知道服务器是否正确得做出了响应，接着就可以做一些事情。比如获取服务器响应的内容，在页面上进行呈现。

```js
//封装ajax请求方法
function request(url, type, fn) {
  //1.创建ajax对象
  var ajax = new XMLHttpRequest()
  //2.与服务器建立连接，type表请求方式，url表请求地址，true表示异步
  ajax.open(type, url, true)
  //3.给服务器发送请求
  ajax.send()
  //4.监听服务器返回数据
  ajax.onreadystatechange = function () {
    //ajax.readyState表请求状态：0，1，2，3，4（请求成功，数据彻底返回）
    if (ajax.readyState === 4) {
      //请求成功，数据彻底返回，拿到返回的数据
      fn && fn(ajax.responseText)
    }
  }
}

//调用request方法
Get.onclick = function () {
  request("/xxx", "GET", function (data) {
    console.log(data)
  })
}
```

## 3.深入 XMLHttpRequest 类

```js
const ajax = new XMLHttpRequest()
/*ajax.readyState表示当前连接状态
  值为0时表示还没建立连接（刚创建出来的时候）
  值为1时表示连接建立成功（调用ajax.open方法之后）
  值为2时表示浏览器开始接收数据（服务器返回的）（ajax.send发送之后陆续向后变化）
  值为3时表示浏览器正在接收数据中
  值为4时表示浏览器接收数据完成
*/
ajax.setRequestHeader //设置请求头(必须写在ajax.open和ajax.send之间)
ajax.status //查看状态码
ajax.statusText //查看状态文本信息
ajax.responseText //响应内容
ajax.getAllResponsetHeaders //获取所有的响应头
```

## 4.jquery 中的 ajax

```js
$.ajax({
  type: "GET",
  url: "service.php?number="("#eyword").val(),
  dataType: "json", //预期服务器数据的类型
  success: function (data) {
    if (data.success) {
      //...
    } else {
      //...
    }
  },
  error: function (jqXHR) {
    //...
  },
})
```

## 5.封装完整的 Ajax 请求函数

```js
//封装完整的ajax请求方法
;(function () {
  //1.适用于任何请求方法
  //2.可以传递参数（请求数据）——get/post
  //3.保证数据类型正确（MIME）
  //4.正确处理响应数据
  const request = {
    //封装创建ajax实例函数
    createAjax() {
      return new XMLHttpRequest()
    },

    //封装转码（将对象 -> xxx=xxx&yyy=yyy形式的字符串）函数
    decode(obj) {
      let arr = []
      for (let i in obj) {
        arr.push(`${i}=${obj[i]}`)
      }
      return arr.join("&")
    },

    //封装get请求方法，参数为url、query传递的参数
    get(url, query, fn) {
      //当不传query参数时，即相当于fn函数相当于query传递
      if (typeof query === "function") {
        fn = query
        query = {}
      }
      //处理query参数对象，调用转码函数
      let strQuery = this.decode(query)
      if (strQuery) {
        //将url -> ?query查询格式
        url = url + "?" + strQuery
      }
      let ajax = this.createAjax()
      ajax.open("GET", url, true)
      //get方法一般不需要传递请求体
      ajax.send(null)
      ajax.onreadystatechange = () => {
        if (ajax.readyState === 4) {
          let response = null
          let ct = ajax.getResponseHeader("Content-Type")
          if (ct === "application/json") {
            response = JSON.parse(ajax.responseText)
          } else {
            response = ajax.responseText
          }
          fn && fn(ajax.responseText, ajax)
        }
      }
    },

    //封装post函数
    post(url, params, fn) {
      //当不传params参数时，即相当于fn函数相当于params传递
      if (typeof params === "function") {
        fn = params
        params = {}
      }
      params = JSON.stringify(params)
      let ajax = this.createAjax()
      ajax.open("POST", url, true)
      ajax.setRequestHeader("Content-Type", "application/json") //设置请求头
      ajax.send(params)
      ajax.onreadystatechange = () => {
        let response = null
        let ct = ajax.getResponseHeader("Content-Type")
        if (ct === "application/json") {
          response = JSON.parse(ajax.responseText)
        } else {
          response = ajax.responseText
        }
        fn && fn(ajax.responseText, ajax)
      }
    },
  }
  //请求对象挂载在window上
  window.request = request
})()
```
