---
title: 移动端rem适配方案
date: 2021/5/14
tags: [rem]
categories: [移动端]
---

## 移动端 rem 适配方案

- `rem.js`
- 在文件入口处引入即可

```js
//仿照手机淘宝团队热门适配方案
;(function flexible(window, document) {
  var docE1 = document.documentElement //返回文档的root元素
  var metaEl = document.querySelector('meta[name="viewport"]') //获取name=viewport的meta对象
  var dpr = window.devicePixelRatio || 1 //获取设备的dpr设备像素比（当前设备下物理像素与虚拟像素的比值）
  //视口缩放比
  var scale = 1 / dpr
  //动态设置meta标签
  metaEl.setAttribute(
    "content",
    `initial-scale = ${scale},maximun-scale = ${scale},minimu-scale = ${scale},user-scalable=no`
  )
  //调整body标签的fontSize = （12 * dpr）px
  //设置默认字体大小
  function setBodyFontSize() {
    if (document.body) {
      document.body.style.fontSize = 12 * dpr + "px"
    } else {
      document.addEventListener("DOMContentLoaded", setBodyFontSize)
    }
  }
  setBodyFontSize()
  //设置1 rem = viewWidth / 10
  function setRemUnit() {
    var rem = docE1.clientWidth / 10 //75px
    docE1.style.fontSize = rem + "px"
  }
  setRemUnit()
  //当页面尺寸大小发生改变时改变rem大小
  window.addEventListener("resize", setRemUnit)
  //当重新加载页面时触发pageshow事件
  window.addEventListener("pageshow", function (e) {
    //e.persisted返回的是true，就是如果这个页面是从缓存取过来的页面，也需要重新计算一下rem
    if (e.persisted) {
      setRemUnit()
    }
  })
})(window, document)
```
