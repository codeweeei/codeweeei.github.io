---
title: Ajax轮询相关踩坑
date: 2021/4/15
tags: [vue实战]
categories: Vue
---

# Ajax 轮询相关踩坑

## axios 轮询

- 两种方案
  - 在 vue 项目中定义一个定时器每隔两秒进行一次 axios 请求
  - 在 vue 项目中当 axios 请求成功返回后再使用延迟器重新请求（推荐）

### 定时器无法清除的坑

- 正常来说清除定时器是在页面销毁的时候清除，如以下代码

```js
this.timer= setInterval(()=>{
  console.log("我刷新了")
},15000)

destroyed(){
	clearInterval(this.timer)
}
```

- 但涉及子组件有时候会失效，此时就需要使用在 `beforeRouteLeave (to, from, next) {}` 离开路由的时候清除

### 完整代码示例

```html
<script>
  export default{
  data(){
      return {
        dataList:[],
        timer:null,
      }
    },
    created(){
      this.sendRequest()
    },
    methods(){
      sendRequest(){
        let that = this;
        if (that.timer != null) {
          window.clearTimeout(that.timer);
        }
        that.$http.get('/url').then(res =>{
          that.dataList = res.data.data
          that timer = setTimeout(()=>{
            that.sendRquest()
          },2000)
        }).catch(err =>{
          throw new Error(err)
        })
      }
    },
    beforeDestroy() {
      if (this.timer) {
        window.clearTimeout(this.timer);
        this.timer = null;
      }
    },
    beforeRouteLeave(to, from, next) {
      console.log("-----关闭维保统计页的定时器  -----");
      if (this.timer != null) {
        clearTimeout(this.timer);
        this.timer = null;
      }
    }
  }
</script>
```
