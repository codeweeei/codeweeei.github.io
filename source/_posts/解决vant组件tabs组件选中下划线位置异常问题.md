---
title: vant组件tabs选中下划线异常问题
date: 2021/3/31
tags: [vue]
categories: Vue
---

## vant 组件 tabs 选中下划线异常问题

- 本文转载[原处](https://my.oschina.net/jamesview/blog/4997187)

- 问题：在使用 vant 中 Tab 标签，点击会显示出现下划线位置异常
- 原因：Tab 挂载的时候会初始化并获取自身宽度，如果处于隐藏状态下，Tab 是获取不到自身宽度的，因此底部条的位置没法正确计算
- 解决方案：在展示 Tab 的时候，调用 Tab 实例上的 resize 方法，使用 watch 监听外层数据变化

- 代码示例：

```html
<div v-show="goodsInfo">
  <div>
    <van-tabs v-model="active" ref="tabs" sticky swipeable animated>
      <van-tab title="商品详情" name="detail">
        <div class="detail" v-html="goodsDetailImg"></div>
      </van-tab>
      <van-tab title="商品评论" name="comment">评论</van-tab>
    </van-tabs>
  </div>
</div>

<script>
  export default {
    watch: {
      goodsInfo(val) {
        this.$refs.tabs.resize()
      },
    },
  }
</script>
```
