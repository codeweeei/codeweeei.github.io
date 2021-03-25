---
title: bcrypt的使用
date: 2021/3/25
tags: [bcrypt, mongoDB]
categories: Node
---

## bcrypt 的使用——对密码加密加盐

### 一：加密加盐

- 安装 bcrypt/bcryptjs
- bcrypt 安装失败可以安装 bcryptjs 来代替
- 建议使用 cnpm 安装
- `cnpm i bcrypt --save`
- 在模型 User.js 里进行加密

```js
// 异步处理（推荐）
const bcrypt = require("bcrypt")
const saltRounds = 10
const myTextPassword = "123456"
const
bcrypt.genSalt(saltRounds, (err, salt) => {
  //生成salt
  bcrypt.hash(myTextPassword, salt, (err, hash) => {
    //将hash存储在密码数据库中
  })
})
```

---

详见[ bcrypt 官网](https://www.npmjs.com/package/bcrypt)
