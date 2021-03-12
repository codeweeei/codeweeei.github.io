---
title: echarts直角坐标系的图表
date: 2021/3/12
tags: [echarts]
categories: 前端数据可视化
---

## 直角坐标系的图表

### 一：横线柱状图

- 例子-横向柱状图

```html
<template>
  <div class="com-container">
    <div class="com-chart" ref="sell_ref"></div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        chartInstance: null,
        allData: null, //服务器返回的数据
      }
    },
    mounted() {
      this.initChart()
      this.getData()
    },
    methods: {
      //初始化echartInstance对象
      initChart() {
        this.chartInstance = this.$echarts.init(this.$refs.sell_ref)
      },
      //获取服务器的数据
      async getData() {
        //获取到数据之后更新图表
        this.updataChart()
      },
      //更新图表
      updataChart() {
        //将返回的数据(对象)转换为两个数组
        const sellerName = this.allData.map((item) => {
          return item.name
        })
        const sellerValue = this.allData.map((item) => {
          return item.Value
        })
        //设置配置项
        const option = {
          xAxis: {
            type: "value", //x轴数值型，不需要设置data，从series里面取
          },
          yAxis: {
            type: "category", //y轴类目型
            data: sellerName,
          },
          series: [
            //系类列表，每个系列通过type决定自己的图表类型 bar为柱型
            {
              type: "bar",
              data: sellerValue,
              //标记出最大值和最小值
              markPoint: {
                data: [
                  {
                    type: "max",
                    name: "最大值",
                  },
                  {
                    type: "min",
                    name: "最小值",
                  },
                ],
              },
              //标记平均值
              markLine: {
                data: [
                  {
                    type: "average",
                    name: "平均值",
                  },
                ],
              },
              //显示数值
              label: {
                show: true,
              },
              //柱的宽度
              barWidth: "50%",
            },
          ],
        }
        //将配置项设置给echarts实例对象
        this.chartInstance.setOption(option)
      },
    },
  }
</script>
```

- 柱状图常见效果配置（在 series 里配置）
  - 标记：
    - markPoint：最大值，最小值
    - markLine：平均值
  - 显示：
    - label 数值显示
    - barWidth 柱宽度
    - 柱状图方向

### 二：折线图

- 使用场景：通常用于每月销量的变化折线
- 使用与柱状图类似，类型为 line
- 折线图常见效果
  - 标记：
    - 最大值，最小值
    - 平均值
    - markArea 标注区间（形成阴影）
  - 线条控制
    - 平滑 smooth
    - 风格样式 lineStyle 对象 线条风格 dashed 虚线，dotted 点线
  - 填充风格：通过 areaStyle 来控制的
  - 紧挨边缘
    - boundaryGap
  - 缩放 scale 脱离 0 值比例(即从 0 点出发)
    - 当数据比较接近时，设置为 true
    - 堆叠图
  - stack 堆叠设置为 all 时当有多条折线时不会出现交叉现象

### 三：地图＋闪点图

- 散点图可以帮助我们推断出变量之间的相关性（正相关，负相关），比如身高和体重的关系
- 数据（二维数组）
  - 数组 1；[[身高1,体重1],[[身高2,体重2]],...]
- 图表类型：
  - series 下设置 `type:scatter`
  - xAxis 和 yAxis 的 type 都要设置为 value
- 例子：身高体重散点图

```html
<html>
  <body>
    <script src="./echarts.min.js"></script>
    <div class="div"></div>
    <script>
      //一维数组数据
      let data = [{ gender: "male", height: 170, weight: 60 }...]
      let axisData = []
      //for循环遍历原始数组数据，取出身高，体重放置在一个数组中，然后将这些一个个数组添加到空数组中，最终形成二维数组
      for(let i =0;i<data.length;i++){
        let height = data[i].heigth
        let weight = data[i].weight
        let newArr = [height,weight]
        axisData.push(newArr)
      }
      let myChart = echarts.init(document.querySelector("div"))
      let option = {
        xAxis:{
          type:"value",
          scale:true
        },
        yAxis:{
          type:"value",
          scale:true
        },
        series:[
          {
            type:"scatter",
            data:axisData
          }
        ]
      }
    </script>
  </body>
</html>
```

- 散点图的常见效果
  - 气泡图的效果
    - 散点的大小不同
      - 通过 symbolSize 来设置（可以使用函数返回大小）
    - 散点的颜色不同
      - 通过 itemStyle.color 来设置
  - 涟漪动画效果
    - 通过 设置 type:effectScatter 即可
    - showEffectOn:'emphasis'来设置鼠标移入才有涟漪动画的效果
    - rippleEffect:{}来设置涟漪动画的范围

### 直角坐标系中的常用配置

- 网格 grid 是用来控制直角坐标系的布局和大小的，x 轴和 y 轴就是在 grid 的基础上进行绘制的
- 坐标轴 axis
  - 坐标轴类型 type
  - 显示位置 position
- 区域缩放 dataZoom
  - 是一个数组，意味着可以配置多个区域缩放器
  - 类型 type
    - slider：滑块
    - inside：内置，依靠鼠标滚轮或者双指缩放
  - 指明产生作用的轴
    - xAxisIndex：设置缩放组件控制的是哪一个 x 轴，一般写 0 即可
  - 指明初始状态的缩放情况
    - start：数据窗口范围的起始百分比
    - end：数据窗口范围的结束百分比

```js
let option = {
  grid: {
    show: true, //开启grid
    borderWidth: 10,
    borderColor: "red",
    left: xx,
    top: xx,
    width: xxx,
    height: xxx,
  },
  xAxis: {
    positon: "top",
  },
  dataZoom: [{}],
}
```

2. 配置 2：坐标轴 axis
3. 配置 3：区域缩放 dataZoom
