---
title: vue引入sass排坑
date: 2021/3/23
tags: [sass,vue实战]
categories: CSS
---

## vue 引入 sass 的排坑

### 一：lang="scss"报错的坑

- 问题：根据 sass 及 vue.js 官网提示安装`npm install -D sass-loader node-sass`后，在 vue 中使用 `lang="scss"`报错；
- 解决方法：此时是 `sass-loader` 的版本过高，推荐使用`@7.3.1` 的版本，而且此时`node-sass`的版本也过高，需要 uninstall 原来的版本，之后下载`@4.14.1`版本，注意可使用 cnpm 下载；

```bash
npm install -D sass-loader@7.3.1
npm install -D node-sass@4.14.1
```

### 二：全局使用 scss 文件的坑

- 问题
  - main.js 可以直接 import css 文件，而不可以直接 import scss 文件
  - 在 index.html 中 link 引入的话，不起作用
  - 每写一个.vue 文件都要在 style 标签里面
    @import “global.scss”这个公共 scss 样式文件来使用公共 scss 样式
- 解决方法：
  - 安装 `sass-resources-loader`
  - `npm install --save-dev sass-resources-loader`
  - 修改 build 中的 utils.js
  ```js
  //将
  scss: generateLoaders("sass")
  //修改成:
  scss: generateLoaders("sass").concat({
    loader: "sass-resources-loader",
    options: {
      resources: path.resolve(__dirname, "../src/assets/global.scss"),
    },
  })
  ```
