---
title: 二：Vue.js的基础语法
date: 2021/1/18
tags: [Vue知识全解]
categories: Vue
---

# Vue 知识全解

## <b style="color:skyblue">二：Vue 基础语法</b>

> 插值语法，动态绑定元素内容

### _1. mustache 语法_

- mustache 语法中不仅可以直接写变量，也可以是简单的表达式
- 语法：双大括号

### _2. v-once 指令_

- 该指令不需要跟任何表达式，表示元素和组件只渲染一次，后面不会随着数据的改变而改变

### _3. v-html 指令_

- 该指令可以解析 html 语句从而渲染出来
- 应用场景：当服务器返回的是以 html 结构的数据时，可以使用该指令；

```js
//<h2 v-html="url"></h2>
data: {
  url: '<a href="www.xxx.com">xxx</a>'
}
```

### _4. v-text 指令_

- 不常用
- 跟 mustache 语法类似，都是用于将数据显示在页面中，会进行覆盖；
- 通常接收一个 string 类型

### _5. v-pre 指令_

- 该指令用于跳过这个元素和它子元素的编译过程，用于显示 原本 的 mustache 内容；

```html
<body>
  <p>{{msg}}</p>
  <!-- 显示xxx -->
  <p v-pre>{{msg}}</p>
  <!-- 显示{{msg}} -->
</body>
<script>
  data: {
    msg: "xxx"
  }
</script>
```

### _6. v-cloak 指令_

- 作用是防止 Vue 解析时卡住（不及时）造成显示出 vue 语法未解析时的样式
- 当 vue 解析完成后 v-cloak 属性会被删除，所以，可以结合样式来使用
- 不常用

```html
<style>
  [v-cloak] {
    display: none;
  }
</style>
<body>
  <div id="app" v-cloak>{{msg}}</div>
</body>
<script>
  //此时Vue延迟一秒解析，所以原始不加v-cloak的话页面会先显示{{msg}}，一秒之后显示xxx，加上v-cloak之后页面会先白屏，再显示xxx
  setTimeOut(function () {
    new Vue({
      el: "#app",
      data: {
        msg: "xxx",
      },
    })
  }, 1000)
</script>
```

> 绑定属性语法

### _1. v-bind 指令动态绑定属性_

- 在需要动态绑定属性的属性前加 v-bind:即可
- 语法糖：: （简写语法）
- v-bind 动态绑定 class 属性
  - （使用多）对象语法：<b style="color:pink">v-bind:class="{类名 1：Boolean,类名 2：Boolean}"</b>,当类名对应的布尔值为 true 时，就会加上该类，而且之前的 class 不会被覆盖，而是进行合并；
  - 数组语法：<b style="color:pink">v-bind:class="['active',line]"</b>，加上''就会代表字符串，不加''就是变量；
- v-bind 动态绑定 style 属性
  - 可以绑定一些 CSS 内联样式，当遇到 font-size 这种样式时，可以使用驼峰 fontSize 命名或'font-size'
  - 对象语法：<b style="color:pink">v-bind:style="{CSS 属性名 1 ：属性值,CSS 属性名名 2：属性值}"</b>,注意属性值添加引号''的区别，不加就会当作变量来解析，加上''就会当作字符串解析；
  - （不常用）数组语法：<b style="color:pink">v-bind:style="[变量]"</b>

```html
<body>
  <div id="app">
    <!-- 如果不加v-bind，imgSrc就会当作单纯的字符串，加了v-bind就会将其解析成data里对应的变量 -->
    <img v-bind:src="imgSrc" />
    <img :src="imgSrc" />
    <!--语法糖简写-->
    <p :class="class"></p>
    <!-- 动态绑定类对象语法：此时该p中的类为active -->
    <p :class="{active:isActive, line:isLine}"></p>
    <!-- 动态绑定样式对象语法：此时该p字体颜色为红色，而且字体大小为100px -->
    <p :style="{color: 'red',fontSize:size + 'px'}"></p>
  </div>
</body>
<script>
  new Vue({
    el: "#app",
    data: {
      imgSrc: "www.xxx.com",
      class: "xxx",
      isActive: true,
      isLine: false,
      size: 100,
    },
  })
</script>
```

### <b style="color:skyblue">_2.计算属性 computed_</b>

- 当需要给数据进行某种变化显示时使用，或者多个数据进行结合显示，起名时使用“属性名”即可，具有缓存机制；
- 计算属性的基本使用

  ```html
  <!-- 计算属性的基本使用 -->
  <div id="app">
    <p>{{firstName}}</p>
    <p>{{firstName}}</p>
    <p>{{fullName}}</p>
  </div>
  <script>
    const app = new Vue({
      el: "#app",
      data: {
        firstName: "Code",
        lastName: "Weeei",
      },
      computed: {
        fullname: function () {
          return this.firstName + "" + this.lastName
        },
      },
    })
  </script>
  ```

- 计算属性的复杂操作

  ```html
  <!-- 计算属性的复杂操作 -->
  <div id="app">
    <p>{{totalPrice}}</p>
  </div>
  <script>
    const app = new Vue({
      el: "#app",
      data: {
        abc: [
          { id: 0, name: "a", price: 100 },
          { id: 1, name: "b", price: 200 },
          { id: 2, name: "c", price: 300 },
        ],
      },
      computed: {
        totalPrice: function () {
          let result = 0
          for (let i in abc) {
            result += i.price
          }
          return result
        },
      },
    })
  </script>
  ```

- 计算属性的 setter 和 getter
  - 计算属性一般没有 set 方法的，只读属性
  - 计算属性完整是一个属性，一般使用里面的 get 方法，所以使用计算属性时不加（）

```html
<!-- computed完整写法 -->
<div id="app">
  <p>{{firstName}}</p>
  <p>{{firstName}}</p>
  <p>{{fullName}}</p>
</div>
<script>
  const app = new Vue({
    el: "#app",
    data: {
      firstName: "Code",
      lastName: "Weeei",
    },
    computed: {
      fullname: {
        //set方法一般不用
        set: function (newValue) {
          。。。
        },
        get: function () {
          return this.firstName + "" + this.lastName
        },
      },
    },
  })
</script>
```

- 计算属性和 methods 的区别
  - 计算属性是基于它们的依赖进行缓存的，只有在相关依赖发生改变时它们才会重新求值也就是说，只要他的依赖没有发生变化，那么每次访问的时候计算属性都会立即返回之前的计算结果，不再执行函数；
  - methods 方法，每当触发重新渲染时，调用方法将总是再次执行函数；
  - 计算属性具有缓存功能，而 methods 没有；

### <b style="color:skyblue">_3.事件监听 v-on_</b>

- v-on 用于绑定事件监听器，缩写@；
- 参数：event
- 基本写法：

```html
<button v-on:click="increment">+</button>
<!-- 语法糖简写 -->
<button @click="increment">+</button>
<script>
  methods:{
    increment(){
      i++;
    }
  }
</script>
```

1. <b style="color:skyblue">v-on 传递参数</b>

- 当通过 methods 中定义方法时，以供@事件调用时，要注意传参的问题：

  - 当事件调用方法没有参数时，可以省略（）
  - 在事件定义时，省略了（），但是方法本身是需要一个参数的，此时事件定义时默认会传入 <b style="color:pink">event</b> 事件参数

  > 当使用 v-on 绑定事件时，省略了（），vue 将浏览器默认生成的 event 事件对象(比如坐标)作为参数传入；

  - 原本如果函数需要参数，但是调用时没有传入，此时形参为 undefined

  ```js
  function abc(name) {
    console.log(name) //undefined
  }
  abc()
  ```

  - 当方法定义时，我们不仅需要传入 event 事件参数，还需要传入其他参数（注意加引号的区别）时，当调用方法时省略参数时，vue 就会将第一个形参作为 event 事件参数传入方法中，之后参数就会变成 undefined：
  - 在调用方法时，如果需要<b style="color:pink">手动传入 event 事件对象参数</b>时，需要在 event 前面加上$即<b style="color:pink">（$event）</b>，否则浏览器会将 event 当作变量传入；

  ```html
  <script src="../js/vue.js"></script>
  <div id="app">
    <button @click="btnClick1(abc,event)">按钮1</button>
    <button @click="btnClick1(bcd,$event)">按钮2</button>
  </div>
  <script>
    const app = new Vue({
      el: "#app",
      data: {
        abc: "abc",
      },
      methods: {
        btnClick1(abc, event) {
          console.log("-----", abc, event) //（报错，event被当成变量） abc,undefined
        },
        btnClick2(bcd, event) {
          console.log("-----", bcd, event) //（报错，bcd当成变量，调用传递参数时加上''就不会将其当成变量） undefined,[event Object]
        },
      },
    })
  </script>
  ```

2. <b style="color:skyblue">v-on 修饰符</b>

- <button @click.stop>btnClick</button>
- <b style="color:pink">.stop</b>：调用 event.stopPropagation()来阻止事件冒泡；
- <b style="color:pink">.prevent</b>：调用 event.preventDefault() 来阻止事件的默认行为；
- <b style="color:pink">.{keyCode | keyAlias}</b>：只当事件是从特定键触发（回车）时才触发回调；
- <b style="color:pink">.native</b>：监听组件根元素的原生事件；
- <b style="color:pink">.once</b>：只触发一次回调；

### <b style="color:skyblue">_4.条件判断 v-if_</b>

1. v-if：

- 原理：v-if 后面的条件为 false 时，对应的元素以及其子元素都不会渲染；也就是根本不会有对应的标签出现在 DOM 中。
- <b style="color:pink">v-if 与 v-show 的区别：</b>
  - 两者都可以决定一个元素是否显示：
  - v-if 当条件为 false 时，对应的元素不会出现在 dom 中；当切换显示和隐藏较少时使用；使用场景：根据服务器传来的数据判断是否渲染；
  - v-show 条件为 false 时，给元素添加 display：none；的行内样式，切换频繁时使用。

### <b style="color:skyblue">_5.遍历数组、对象和组件 v-for_</b>

- v-for 中的 <i style="color:pink">key</i> 的作用：
  - key 作为每个节点的唯一标识；
  - 可以提升 v-for 渲染的效率性能：vue 不会改变原有的元素和数据，而是创建新的元素然后将新的数据渲染进去，而从高效的更新虚拟 DOM。

```html
<li v-for="{item, index} in items" :key="item"></li>
```

- for 循环
  ```js
  for (let i = 0; i < items.length; i++) {
    //普通for循环
  }
  for (let i in items) {
    //用于for-in循环得来item的索引值
  }
  for (let item of items) {
    //用于for-of循环直接得到item
  }
  ```
- <b style="color:pink">vue 中 哪些数组方法不是响应式的：</b>
  - 直接通过数组下标来改变数组的方法不是响应式的；（推荐 splice 方法来修改数组）

### <b style="color:skyblue">_6.表单双向绑定 v-model_</b>

- 表单控件在实际开发中是比较常见的，特别是对于用户信息的提交；
- v-model 的基本使用：

```html
<body>
  <div id="app">
  <input type="text" v-model="message"/>
  {{message}}
  <!-- 当input使用v-model，会进行双向绑定的修改（message改变input输入框的value，value反过来改变message） -->
  </div>
  <script src=../vue.js></script>
  <script>
  const app = new Vue({
    el: "#app",
    data: {
      message:"xxx",
    }
  })
  </script>
</body>
```

- 通过 v-bind 和 v-on 绑定 input 事件来手动设置一个 v-model：

```html
<input type="text" v-bind:value="message" v-on:input="valueChange" />
<!-- oninput事件表示元素获得用户输入时运行 -->
<!-- 简写 -->
<input
  type="text"
  v-bind:value="message"
  v-on:input="message = $event.target.value"
/>

<script>
  const app = new Vue({
    el: "#app",
    data: {
      message: "xxx",
    },
    methods: {
      valueChange(event) {
        this.message = event.target.value
      },
    },
  })
</script>
```

- v-model 的值绑定：就是 v-bind 在 input 结合 v-model 的使用，真实开发中，我们是不能直接写死数据的，需要从服务器获取到数据，然后再通过值绑定数据；
- <b style="color:pink">v-model 修饰符的使用：</b>
  - 默认情况下：v-model 默认是在 input 事件中同步输入框的数据的，也就是说一旦有数据发生改变对应的 data 数据也会发生改变（默认 v-model 给 data 赋值为 string 类型）；
  - laze 修饰符：可以让数据在失去焦点或者回车时才更新；
  - number 修饰符：当加上.number 修饰符时，并且结合数字输入框时，v-model 给 data 赋值为 number 类型；
  - trim 修饰符：当字符串左右两边很多空格时，使用.trim 可以过滤掉左右两边的空格；
