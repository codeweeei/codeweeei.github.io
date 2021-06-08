---
title: uniapp项目app与H5端高度设置问题
date: 2021/5/21
tags: [uni-app常见Bug]
categories: uni-app
---

## uniapp 项目 H5 端与 app 端高度设置问题

### app 端高度使用%失效

- h5 端设置 page 高度

```css
page {
  width: 100%;
  height: 100%;
}
```

- 设置好以后在 H5 端生效，连接手机进行真机调试，page 高度失效，目前解决方案是给页面一个`view class="wrapper"`，然后再 css 中设置

```css
.wrapper {
  width: 100%;
  height: 100vh;
}
```

> 但是使用 vh 就会出现另一个问题

### app 端与 H5 端 vh 高度不同

- H5 端中使用 vh 是整个可见区域的高度为 100vh
- app 端使用 vh 是减去顶部导航栏与底部 tabbar 的高度之后整个可见区域为 100vh

### 解决方案：使用条件判断#ifdef

- 实例代码

```css
.wrapper {
  width: 100%;
  /* #ifdef H5 */
  height: calc(100vh - 88rpx - 100rpx);
  /* #endif */
  /* #ifdef APP-PLUS */
  height: 100vh;
  /* #endif */
}
```
