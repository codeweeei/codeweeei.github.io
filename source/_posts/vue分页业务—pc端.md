---
title: vue分页业务——pc端
date: 2021/3/20
tags: [vue实战]
categories: Vue
---

## vue 实现分页功能——pc 端

- 需要后端返回总的数据（页数，条数）
  - 后端根据前端传来的每页的条数来根据总的条数计算出返回的总的页数

> 后端数据例子

```json
// http://IP:HOST/home/1/30
{
  "data": [
    {
      // 类似该种情况，总共270条数据，客户传来第一页，显示30条数据
    }
  ],
  "rows": "270",
  "pages": "9"
}
```

### 步骤

1. 从后端获取 data 数据（数据，总页数，总数据条数）

2. 展示（通过表格来展示）

- 使用 v-for 渲染
- 在页面 mounted 时调用获取第一页数据接口的方法（封装成 getData 方法函数，后续可以进行复用）

3. 点击页码来展示不同页的数据

- 使用 v-for 遍历循环总的页码信息
- 点击页码时获取当前页码信息，在 data 里定义当前所在的页码，点击时添加点击方法，将当前页码索引值作为参数，点击调用将 data 中的当前所在的页码数设置为当前索引
  - 注意第一次生成页面时，不需要传入页码参数信息，可以判断一下是否有传来的页码参数，有则将当前页码设置为页码参数，否者直接用原始的页码（1），或者使用 es6 的传入默认值 i=1

4. 点击跳转上一页以及下一页的跳转

- 下一页：判断当当前页码小于总的页码数时，将当前页码进行自增 1，然后调用获取数据的方法，将当前页码传入
  - 下一页到底时按钮设置为不可点（禁用）
- 上一页：判断当当前页码大于 1 时，将当前页码进行自减 1，然后调用获取数据的方法，将当前页码传入
  - 上一页到底时按钮设置为不可点（禁用）

5. 对分页（页码数多时）添加省略号

- 省略号的添加是分页大于 10 的时候
- 临界开始情况：当前页码小于等于 5 时点击，返回[1,2,3,4,5,6,7,'...',20]
- 中间情况：返回[1,'...',当前页-n,当前页,当前页+n,'...',20]
- 临界边缘情况：当前页码为大于等于最后一页-5 时点击，返回[1,'...',15,16,17,18,19,20]
- 在 computed 计算属性里操作

```js
computed(){
  pages(){
    var start = this.pageNow, end = this.pages
    if(end < 20){
      return end
    }
    if(start <= 5){
      return [1,2,3,4,5,6,7,'...',20]
    }
    。。。
  }
}
```

> 实例代码

```html
<template>
  <div>
    <table>
      。。。
    </table>
  </div>
</template>
<script>
  data(){
    return {
      lists:[],//展示表格数据
      pages:1,//总页数
      rows:1，//总条数
      pageNow:1//当前页
      pageSize:10//每一页的条数
    }
  },
  method:{
    //获取数据
    getData(i){
      this.pageNow = i || this.pageNow
      axios({
        method:'get',
        url:`http://IP:HOST/home/page/$
        {this.pageNow}/${this.pageSize}`
      }).then( res => {
        //结构赋值
        let {data,pages,rows} = res.data
        this.lists = data
        this.pageTotal = pages
        this.rows = rows
      })
    },
    //点击分页页码按钮进行跳转，传入当前页码数
    curpage(i){
      this.getData(i)
    }
    //点击下一页的跳转
    nextPage(){
      if(this.pageNow < this.pages){
        this.getData(++this.pageNow)
      }
    },
    //点击上一页的跳转
    lastPage(){
      if(this.pageNow > 1){
        this.getData(--this.pageNow)
      }
    }
  }
  ,
  mounted(){
    this.getData()
  }
</script>
```
