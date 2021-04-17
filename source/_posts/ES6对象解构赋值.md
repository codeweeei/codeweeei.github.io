---
title: ES6对象解构赋值
date: 2021/4/17
tags: [ES6]
categories: Javascript
---

## ES6 对象的解构赋值

### 解构赋值

- 解构赋值语法是一个 Javascript 表达式，这使得可以将数据从数组或对象提取到不同的变量中(这段话是 mdn 中关于解构赋值的定义，注意这里的定义，可以看出解构主要用在数组和对象上)。

- 就是解析等号两边的结构，然后把右边的对应赋值给左边（如果解构不成功，变量的值就等于
  undefined）

### 常见的使用场景

- 处理后端返回的 json 数据，取出自己想要的字段值
- 注：json 结构和键名要一一对应才能进行正确解构赋值

```js
let dataJson = {
  title: "abc",
  name: "winne",
  test: [
    {
      title: "ggg",
      desc: "对象解构赋值",
    },
  ],
}
//我们只需要取出需要的两个title值（注意结构和键名）
let {
  title: oneTitle,
  test: [{ title: twoTitle }],
} = dataJson
//如果只需要其中一个数据，直接根据结构键名来写就好了
let { name } = dataJson //相当于es5的 let name = dataJson.name;
console.log(oneTitle, twoTitle) // abc  ggg
console.log(name) // winne
```
