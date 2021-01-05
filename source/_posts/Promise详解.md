---
title: Promise详解
date: 2021/1/6
tags: [Promise]
categories: Javascript
---

## promise 详解

### <b style="color:pink">一：为什么需要 promise</b>

- 需求：通过 ajax 请求 id，再根据 id 请求用户名，再通过用户名获取邮箱...
- 代码实现

```json
{
  //data1.json
  "id": 1
}
```

```json
{
  //data2.json
  "username": "超威"
}
```

```json
{
  //data3.json
  "email": "xxxx.@qq.com"
}
```

```js
//发送Ajax请求获取id
$.ajax({
  type: "GET",
  url: "./data1.json",
  success: function (res) {
    //里面的代码为异步调用
    //res 表示返回的结果
    const { id } = res //对象的结构赋值，将返回的结果中id的值赋值在id中
    console.log(id) //1
    console.log(res) //正常打印data1.json里的数据
    $.ajax({
      type: "GET",
      url: "./data2.json",
      data: { id },
      success: function (res) {
        console.log(res) //正常打印data2.json里的数据
        const { username } = res
        $.ajax({
          type: "GET",
          url: "./data3.json",
          data: { username },
          success: function (res) {
            console.log(res)
          },
        })
      },
    })
  },
})
// //再发送一次Ajax请求通过id获取username
// $.ajax({
//   type: "GET",
//   url: "./data2.json",
//   data: { id },
//   success: function (res) {
//     console.log(res) //报错id未定义，因为该代码顺序为先执行第一个$.ajax（同步），再执行第二个$.ajax（同步），之后再执行异步代码，故获取不到id的值；
//   },
// })
```

> 回调地狱的问题，如上述代码可见若多层嵌套发送 Ajax 请求会造成回调地狱（在回调函数中嵌套回调）的问题，代码不美观，且不易维护；

### <b style="color:pink">二：Promise 的基本使用</b>

- promise 是一个构造函数，通过 new 来实例化对象，promise 接收一个函数作为参数，参数函数中接收 resolve 和 reject 两个形参；
- 基本语法：

```js
const promise = new Promise((resolve, reject) => {})
```

- promise 实例有两个属性
  - state：状态
    - 第一种状态：pending（准备，待解决，进行中）
    - 第二种状态：fulfilled（已完成，成功）
    - 第三种状态：rejectd（已拒绝，失败）
    - 状态的改变<span   style="color:#e88565">（只能改变一次状态）</span>
      - 通过调用 resolve（）<span   style="color:skyblue">【pending -> fulfilled】</span>和 reject（） <span style="color:skyblue"> 【pending -> rejectd】</span>函数来 改变当前 promise 实例的状态；
  - result：结果
    - 通过给调用 resolve 函数，传递参数，来改变当前 result 的结果；
    - 通过给调用 reject 函数，传递参数，来改变当前 result 的结果；
  ```js
  const promise = new Promise((resolve, reject) => {
    resolve("成功的结果")
  })
  ```

### <b style="color:pink">三：promise 原型里的方法</b>

1. then 方法

- then 方法也传递两个参数函数；
- 当 promise 的状态为 fulfill 时执行第一个参数；
- 当 promise 的状态为 rejected 时执行第二个参数；
- then 方法的返回值也是一个 新的 promise 实例对象，状态为 pending；
  > 注意需要使用断点调试模拟代码真实执行顺序（同步异步）才可得到该真实结果（pending），不使用断点的话返回的新 promise 实例的状态为 fulfilled
  - 即可采用链式操作，使代码更加扁平化
- 通过 then 方法第一个参数函数里的参数
  可以得到 resolve 函数传递的参数数据；
- 通过 then 方法第二个参数函数里的参数
  可以得到 reject 函数传递的参数数据；

```js
const $http = new Promise((resolve, reject) => {
  resolve("成功了")
  // reject("出错了")
})
$http.then(
  (res) => {
    //当promise的状态为fulfill时执行
    console.log(res) //成功了
  },
  (err) => {
    //当promise的状态为reject时执行
    console.log(err) //出错了
  }
)
```

> 如果 promise 的状态不改变（不调用 resolve 或 reject 函数），就不会执行 then 里面的方法，使用 return 语句可以将 then 方法返回的实例状态改为 fulfilled；如果在 then 方法中出错，会将返回的 t 实例状态改为 rejected

```js
const p = new Promise((resolve, reject) => {
  resolve()
})
const t = p.then(
  (res) => {
    console.log("成功1") //打印
    //使用return可将t实例的状态改为fulfilled
    return 123
    //如果在then方法中出错，会将返回的t实例状态改为rejected
    // console.log(a)
  },
  (err) => {
    console.log("出错1")
  }
)
const w = t.then(
  (res) => {
    console.log("成功2", res)
  },
  (err) => {
    console.log("出错2", err) //打印
  }
)
```

2. catch 方法

- catch 方法中的参数函数在什么时候执行？
  - 当 promise 的状态改为 rejected 时执行；
  - 当 promise 执行体中出现代码错误（人为抛出错误也行）时执行，并可捕获到该错误；

```js
const p = new Promise((result, reject) => {
  //1.promise状态改为rejected时
  reject("出错了")
  //2.执行出现错误时
  // console.log(a)
  //3.人为抛出错误
  // throw new Error('出错了')
})
p.catch((err) => {
  console.log(err)
})
```

### <b style="color:pink">四：promise 常规写法</b>

- 代码

```js
new Promise((resolve, reject) => {
  ...
}).then((res) => {
  //成功时执行
}).catch((err) => {
  //出错时执行
})
```

### <b style="color:pink">五：使用 promise 解决回调地狱 </b>

- 示例

```js
const id = new Promise((resolve, reject) => {
  $.ajax({
    type: "GET",
    url: "./data1.json",
    success: function (res) {
      resolve(res)
    },
    error: function (err) {
      reject(err)
    },
  })
})
  .then((res) => {
    const { id } = res //ES6解构赋值
    console.log(id) //1
    //使用return将返回的promise状态改为fulfilled，并且根据获取的id值继续发送ajax请求
    return new Promise((resolve, reject) => {
      $.ajax({
        type: "GET",
        url: "./data2.json",
        data: { id },
        success: function (res) {
          resolve(res)
        },
        error: function (err) {
          reject(err)
        },
      })
    })
  })
  .then((res) => {
    const { username } = res
    console.log(username) //超威
    //综上
    return new Promise((resolve, reject) => {
      $.ajax({
        type: "GET",
        url: "./data3.json",
        data: { username },
        success: function (res) {
          resolve(res)
        },
        error: function (err) {
          reject(err)
        },
      })
    })
  })
  .then((res) => {
    const { email } = res
    console.log(email) //xxxx@qq.com
  })
```

### <b style="color:pink">六：优化代码 </b>

- 上述的代码虽然结构清晰，但是有太多相视的代码，故可以封装优化一下；

```js
//封装多余的代码成一个函数，返回promise对象
function GetData(url, data = {}) {
  return new Promise((resolve, reject) => {
    $.ajax({
      type: "GET",
      url: url,
      data: data,
      success: function (res) {
        resolve(res)
      },
      error: function (err) {
        reject(err)
      },
    })
  })
}

GetData("data1.json")
  .then((res) => {
    const { id } = res //ES6解构赋值
    console.log(id) //1
    //使用return将返回的promise状态改为fulfilled，并且根据获取的id值继续发送ajax请求
    return GetData("data2.json", id)
  })
  .then((res) => {
    const { username } = res
    console.log(username) //超威
    return GetData("data3.json", username)
  })
  .then((res) => {
    const { email } = res
    console.log(email) //xxxx@qq.com
  })
```

> 简单来说，Promise 就是用同步的方式写异步的代码，用来解决回调问题；
