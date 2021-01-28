---
title: 三：Vuejs组件化开发
date: 2021/1/22
tags: [Vue知识全解]
categories: Vue
---

# Vue 知识全解

## <b style="color:skyblue">三：Vue 组件化开发</b>

- 组件化思想：如果将一个页面中所有处理逻辑全部放在一起，处理起来会非常复杂，而且不利于后续的管理以及扩展；但如果将一个页面拆分为一个个小的功能块，每个功能块完成属于自己这部分独立的功能，那么同样能实现整个页面的完整功能，并且之后的管理和维护也能变得更加简单。
- vue.js 中的组件化开发思想：我们可以利用组件化思想来开发一个个可以独立复用的小组件来构造我们的应用，任何的应用都可以抽象成一个组件树。

### _1.注册组件的基本步骤_

- 创建组件构造器
  - 调用 Vue.extend()方法创建组件构造器
- 注册组件
  - 调用 Vue.component()方法来注册组件
  - 全局注册
  - 局部注册
- 使用组件

  - 在 Vue 实例的作用范围内使用组件

- vue 组件化开发的基本使用

```html
<body>
  <div id="app">
    <p>----原始----</p>
    <h2>标题</h2>
    <p>我是内容1</p>
    <p>我是内容2</p>
    <p>----组件化----</p>
    <!-- 3.使用组件（在new Vue挂载的el上使用） -->
    <my-cpn></my-cpn>
    <my-cpn></my-cpn>
  </div>
  <t src="../js/vue.js"></t>
  <t>
    //1.创建组件构造器 const MyCpn = Vue.extend({
    //模板属性：template（就是自定义组件的模板，就是在使用到组件的地方，要显示的HTML代码）
    template: `
    <div>
      <h2>标题</h2>
      <p>我是内容1</p>
      <p>我是内容2</p>
    </div>
    `, }) //2.注册组件 Vue.component("my-cpn", MyCpn) const app = new Vue({ el:
    "#app", data: { message: "xxx", }, })
  </t>
</body>
```

### _2.全局组件和局部组件_

- 全局组件：就是可以在多个 Vue 实例下使用；
  - 使用 Vue.component 注册的即是全局组件(如上述例子)
- 局部组件：在创建的 Vue 实例里注册的组件就是局部组件，只能在该实例挂载的 dom 节点里使用；

  ```js
  const app = new Vue({
    el: "#app",
    data: {},
    components: {
      "my-cpn": MyCpn,
    },
  })
  ```

### _3.父组件和子组件_

- 在组件构造器（父组件）里使用 components 属性注册子组件：

```js
const cpn2 = Vue.extend({
  template: `
        <h2>我是子组件</h2>
      `,
})
const cpn1 = Vue.extend({
  //注意当模板里有多个标签时，最外边需要一个根标签
  template: `
        <div>
          <h2>我是父组件</h2>
          <cpn2></cpn2>
        </div>          
      `,
  //注册子组件
  components: {
    cpn2,
  },
})
//根组件
const app = new Vue({
  el: "#app",
  components: {
    cpn1,
  },
})
```

### _4.注册组件的语法糖_

- 可以省去 Vue.extend()组件构造器的环节，直接使用一个对象来替代

```js
//vue内部会调用 Vue.extend()组件构造器
//1.全局注册组件的语法糖形式：
Vue.component("cpn1", {
  templete: `
      <div>
        <h2>xxx</h2>
      </div>  
    `,
})
//2.局部注册组件的语法糖形式：
const app = new Vue({
  el: "#app",
  components: {
    cpn2: {
      templete: `
      <div>
        <h2>yyy</h2>
      </div>  
    `,
    },
  },
})
```

### _5.组件模板抽离的写法_

- 之前的 template 都要写在``里，看起来不太美观，而且结构比较乱，而且没有代码提示，所以我们可以将其抽离出来；
- 方法 1：使用 script 标签 抽离
  ```html
  <script type="text/x-template" id="cpn">
    <div>
      <h2>xxx</h2>
      <p>xxx</p>
    </div>
  </script>
  <script>
    Vue.component("cpn", { template: "#cpn" })
  </script>
  ```
- 方法 2：使用 template 标签 抽离
  ```html
  <template id="cpn">
    <div>
      <h2>xxx</h2>
      <p>xxx</p>
    </div>
  </template>
  <script>
    Vue.component("cpn", {
      template: "#cpn",
    })
  </script>
  ```

### _6.组件访问 vue 实例数据_

- 组件是一个单独功能模板的封装：这个模板有属于自己的 HTML 模板，也应该有属于自己的数据 data；
- 组件是不可以直接访问到根 Vue 实例 data 里的数据；组件应该有属于自己保存数据的地方(data)。
  - <b style="color:pink">组件里的 data 必须是一个函数并且返回一个对象</b>
    - 原因：组件里的 data 如果不是一个函数，是一个对象的话，就会造成多个组件直接共同使用该 data 对象里的数据；如果是函数，而且返回对象的话，组件每次调用 data 函数时就会返回一个新的对象，这样就不会造成多个组件直接数据混乱的现象。
    - 概括：每次函数执行都会返回一个全新的 data 对象，若不是函数（会报错），则返回同一个 data 对象，所以在多组件中则会造成状态污染
    - vue 的根实例没有这个限制，因为根实例只有一个，所以不存在这种情况

```html
<div id="app">
  <cpn></cpn>
</div>
<template id="cpn">
  <div>
    <h2>我是标题</h2>
    <p>{{message}}</p>
  </div>
</template>
<script src="../js/vue.js"></script>
<script>
  Vue.component("cpn", {
    template: "#cpn",
    data() {
      return {
        message: "我是内容2",
      }
    },
  })
  const app = new Vue({
    el: "#app",
    data: {
      message: "我是内容1",
    },
  })
</script>
```

### _7.组件之间的通信_

> 1.父子组件的通信

- 首先子组件是不可以直接访问父组件或者根 Vue 实例里的数据的；
- <b style="color:pink">父传子</b>

  - 使用场景：在一个页面中，从服务器里获取了很多数据，通常不会在页面中的大组件进行显示这些数据，而是通过将获取到的数据传给下面的子组件进行显示。
  - 通过 <b style="color:skyblue">props</b> 向子组件传递数据
  - 使用方式：父组件调用子组件时使用 v-bind 传递数据，子组件使用 props 接收父组件传递的数据
  - props 的值有两种形式：

    - 形式一：字符串数组，数组中的字符串就是传递时的名称(用的少)。

    ```html
    <!-- 例子 -->
    <cpn :cmessage="message" :cmovies="movies"></cpn>
    <script>
      const cpn = {
        template: "#cpn",
        props: ["cmessage", "cmovies"],
      }
    </script>
    ```

    - 形式二：对象，对象可以设置传递时的类型（String，Number，Boolean，Array，Object，Function，Date，Symbol），也可以设置默认值，以及是否必传，，当有自定义构造函数时，甚至可以验证自定义的类型；

      - 其中 Object 和 Array 类型的，默认值必需是一个函数，再返回需要的对应类型的数据

    ```html
    <!-- 例子 -->
    <cpn :cmessage="message" :cmovies="movies"></cpn>
    <script>
      const cpn = {
        template: "#cpn",
        props: {
          cmovies: {
            type: Array,
            default(){
              return ["xxx"]
            },
            required: false,
          }
          cmessage: {
            //类型限制
            type: String,
            //设置默认值，父组件不传时就默认有的
            default: "aaaaa",
            //required为true时，如果父组件不传时就会报错
            required: true,
          },
        },
      }
    </script>
    ```

  - props 中的驼峰标识：当 props 用驼峰标识符时，v-bind 不建议使用驼峰标识符，可以在中间加个<b style="color:pink"> - </b>来连接；

- <b style="color:pink">子传父</b>

  - 使用场景：当子组件进行用户交互后更新了数据，需要发送给父组件。
  - 通过 <b style="color:skyblue">自定义事件 emit</b> 向父组件传递数据
  - 使用：子组件使用 this.$emit("事件",参数)来向父组件发射自定义事件，父组件使用 v-on(@)事件来接收事件，注意事件尽量不要使用驼峰标识，可使用-代替；

  ```html
  <!-- 父组件 -->
  <div id="app">
    <!-- 注意不要写驼峰标识 -->
    <cpn @item-click="showItem"></cpn>
  </div>
  <!-- 子组件 -->
  <template id="cpn">
    <div>
      <button
        v-for="item in categories"
        :key="item"
        @click="btnClick(item)"
      >
        {{item.name}}
      <button>
    </div>
  </template>
  <script>
    Vue.component("cpn",{
      template: "#cpn",
      methods: {
        btnClick(item){
          //发送事件
          this.$emit("item-click",item)
        }
      }
    })
    const app = new Vue({
      el: "#app",
      data: {
        categories:{
          [id:"1",name:"家电"],
          [id:"2",name:"水果"],
          [id:"3",name:"数码"],
        }
      },
      components: {
        cpn,
      }
      methods: {
        showItem(item){
          console.log(item.name)
        }
      }
    })
  </script>
  ```

- 父访问子 children-refs
  - 父组件直接访问子组件
  - $children（数组类型）
    - 父组件调用 this.$children[0]拿到第一个子组件 （注意是数组对象）（不常用）
  - $refs（对象类型）
    - 可以在某个组件加上 ref="xxx"属性，然后通过 this.$refs.xxx 来得到该组件（常用）
- 子访问父 parent（不常用）
  - 子组件直接访问父组件（不建议使用，耦合度太高）
  - $parent
  - $root 直接访问根组件 Vue 实例

### _8.插槽 slot 的使用_

- 组件的插槽
  - 组件的插槽可以为了让我们封装的组件更具有扩展性
  - 可以让使用者决定组件内部的一些内容到底展示什么
  - 例如移动开发中的页面导航栏，每个页面的导航栏都是可能不同的，就可以预留插槽来决定每个页面的导航栏的展示；
- 如何封装：
  - 抽取共性，保留不同
  - 将共性抽取到组件中，将不同暴露在插槽中
- 插槽的基本使用：
  ```html
  <div id="app">
    <cpn>
      <!-- 这里的button（全部）都会覆盖掉子组件slot的位置 -->
      <button>插槽的使用</button>
      <button>插槽的使用</button>
    </cpn>
  </div>
  <template id="cpn">
    <div>
      <h2>标题</h2>
      <p>内容</p>
      <slot>
        <!-- 这里可以设置slot的默认值，会被覆盖掉 -->
      </slot>
    </div>
  </template>
  ```
- 具名插槽的使用

  - 当子组件的功能比较复杂时，子组件的插槽的可能并非只有一个，那么此时如何区分插入的内容是对应哪一个插槽呢？
  - 使用具名插槽：给 slot 元素加上 name 属性

  ```html
  <div id="app">
    <cpn>
      <!-- 该按钮会替换掉name="left"的插槽 -->
      <button slot="left">左</button>
      <!-- 该按钮会替换掉name="right"的插槽 -->
      <button slot="right">右</button>
    </cpn>
  </div>
  <template id="cpn">
    <div>
      <h2>标题</h2>
      <p>内容</p>
      <slot name="left"></slot>
      <slot name="center"></slot>
      <slot name="right"></slot>
    </div>
  </template>
  ```

- 作用域插槽

  - 编译作用域
    - 父组件模板（vue 实例）的所有东西都会在父级作用域内（vue 实例）编译；子组件模板的所有东西都会在子级作用域内编译。
  - 作用域插槽的使用：

    - 父组件替换插槽里面的标签，但是内容由子组件来提供的

    ```html

    <div id="app">
    <!-- 2.<template slot-scope="slotProps">获取到slotProps属性 -->
      <cpn>
        <template slot-scope="slotProps">
          <!-- 3.通过slotProps.datas就可以获取到刚才我们传入的datas了-->
          <ul>
            <li v-for="item in slotProps.datas" :key=item>{{item}}</li>
          </ul>
        <template>
      </cpn>
    </div>

    <template id="cpn">
      <div>
        <!-- 1.将子组件的message绑定到datas里 -->
        <slot :datas="messages"></slot>
      </div>
    </template>

    <script>
    //子组件中包括一组数据
      Vue.component("cpn",{
        template: "#cpn",
        data(){
          return {
            messages:['a','b','c']
          }
        }
      })
    </script>
    ```

### _9. v-slot 插槽指令 的使用_

> 在 2.6.0 中，vue 为具名插槽和作用域插槽引入了一个新的统一的语法 (即 v-slot 指令)。目前 slot 与 slot-scope 这两个特性暂时还未被移除。

- 具名插槽

```js
// 组件
Vue.component("lv-hello", {
  template: `
    <div>
      <slot name="header"></slot>
      <h1>我的天呀</h1>
    </div>
  `,
})
```

```html
<div id="app">
  <!-- 老版本使用具名插槽 -->
  <lv-hello>
    <p slot="header">我是头部</p>
  </lv-hello>
  <!-- 新版本使用具名插槽 -->
  <lv-hello>
    <!-- 注意：这块的 v-slot 指令只能写在 template 标签上面，而不能放置到 p 标签上 -->
    <template v-slot:header>
      <p>我是头部</p>
    </template>
  </lv-hello>
</div>
```

- 具名插槽的缩写
  > 将 v-slot: 替换成 # 号

```html
<div id="app">
  <lv-hello>
    <template #header>
      <p>我是头部</p>
    </template>
    <!-- 注意: #号后面必须有参数，否则会报错。即便是默认插槽，也需要写成 #default -->
    <template #default>
      <p>我是默认插槽</p>
    </template>
  </lv-hello>
</div>
```

- 作用域插槽
  > 所谓作用域插槽，就是让插槽的内容能够访问子组件中才有的数据。

```js
Vue.component("lv-hello", {
  data: function () {
    return {
      firstName: "张",
      lastName: "三",
    }
  },

  template: `
    <div>
      <slot name="header" :firstName="firstName" :lastName="lastName"></slot>
      <h1>我的天呀</h1>
    </div>
  `,
})
```

```html
<div id="app">
  <!-- 老版本使用具名插槽 -->
  <lv-hello>
    <p slot="header" slot-scope="hh">
      我是头部 {{ hh.firstName }} {{ hh.lastName }}
    </p>
  </lv-hello>
  <!-- 新版本使用具名插槽 -->
  <lv-hello>
    <!-- 注意：这块的 v-slot 指令只能写在 template 标签上面，而不能放置到 p 标签上 -->
    <template v-slot:header="hh">
      <p>我是头部 {{ hh.firstName }} {{ hh.lastName }}</p>
    </template>
  </lv-hello>
</div>
```
