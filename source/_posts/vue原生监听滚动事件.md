---
title: vue原生监听滚动事件
date: 2020/12/12
tags: vue实战
categories: Vue
---

## _vue 原生监听滚动事件及实现点击回到顶部_

```js
  //添加监听事件
  mounted() {
    window.addEventListener('scroll',this.handleScroll, true)
  },
  methods: {
    //监听滚动事件
    handleScroll() {
      let scrollTop = window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop
      // console.log(scrollTop)
      if(scrollTop>=188){
        //当滚动到超过一定距离后，回到顶部按钮显示（结合v-show）
        this.showheight=true
      }else{
        this.showheight=false
      }
    },
    //点击回到顶部事件
    totopClick() {
      let top = document.documentElement.scrollTop || document.body.scrollTop || window.pageYOffset
      //实现滚动效果，定时滚动,实现动画效果
      const timeTop = setInterval(() => {
        document.body.scrollTop = document.documentElement.scrollTop = top -= 50
        if (top <= 0) {
          clearInterval(timeTop)
        }
      }, 15);
    }
  },
  //销毁监听滚动
  destroyed() {
    // 离开该页面需要移除这个监听的事件，不然会报错  必须带第三个参数true，否则销毁不成功
    window.removeEventListener('scroll', this.handleScroll, true)
  }
```
