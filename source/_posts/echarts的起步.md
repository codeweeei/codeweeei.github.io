---
title: echarts的起步
date: 2021/3/12
tags: [echarts]
categories: 前端数据可视化
---

## echarts

1. 组件结构的设计

- SellerPage.vue 用于测试用,针对于/sellerpage 这条路径而显示出来的,在这个组件中,通过子组件注册的方式,要显示出 Seller.vue 这个组件
- Seller.vue 使用 echarts,来呈现出相关图表

2. 布局结构的设计
3. 图表基本功能的实现

- 图表实现的一般步骤
  1. initChart
     - 初始化 echartsInstance 对象
  2. getData
     - 获取服务器数据
  3. updataChart
     - 更新图表,设置 option

4. 动态刷新的实现
5. UI 的调整
6. 拆分图表的 option
7. 分辨率适配
8. 通用配置：即任何图表都能使用的配置

   - title：标题
     - 文字样式 textStyle
     - 标题边框 boderWidth、borderColor、borderRadius
     - 标题位置 left、right、top、bottom
   - tooltip：鼠标滑过之后的提示
     - 触发类型：trigger
     - 触发时机：triggerOn 鼠标移入，鼠标点击
     - 格式化：formatter
   - toolbox：echarts 提供的按钮工具栏
     - 内置有导出图片，数据视图，动态类型切换，数据区域缩放，重置五个工具

   ```js
   toolbox: {
     feature: {
       //导出图片的功能
       saveAsImage: {},
       //数据视图
       dataView:{},
       //刷新重置
       restore:{},
       //区域缩放
       dataZoom:{},
       //图表之间动态切换
       magicType:{
         type:['bar','line']
       },
     }
   }
   ```

   - legend：图例
     - 用于筛选系列，需要和 series 配合使用
     - 当有多个系类 series 时，使用 legend 中的 data 数组与 series 里的 name 对应
