---
title: Js原型与继承
date: 2020/12/18
tags: [前端面试, 原型, 继承]
categories: Javascript
---

<i style="font-size:13px">该文章引用自后盾人教程，原处[链接](https://houdunren.gitee.io/note/js/11%20%E5%8E%9F%E5%9E%8B%E4%B8%8E%E7%BB%A7%E6%89%BF.html#%E5%8E%9F%E5%9E%8B%E5%9F%BA%E7%A1%80)，点击跳转，感谢向军老师！！！</i>

---

## 原型

### 1.理解：每个对象都有一个原型 \_proto\_ 对象，通过函数创建的对象也将拥有这个原型对象。原型是一个指向对象的指针。

- 原型可以理解为相当于对象的父亲，对象从原型对象继承来属性；
- 对象方法优先于原型里的方法；
- 函数拥有多个原型，即函数即可当作对象（\_proto\_），也可当作构造函数（prototype）来创建对象；
- 对象的\_proto\_指向构造函数的 prototype；
- constructor

  - 每个原型都有一个 constructor 属性，指向该关联的构造函数。

  ```js

    //注意直接给构造函数以对象形式设置原型时要
    加上constructor: User,以防constructor丢
    失；

    User.prototype = {

      constructor: User,

      show: function () {},

    }

  ```

### 2.创建对象的方法

- 字面量创建
- 构造函数创建

  - Js 内置构造函数：1，Number();
    2，String();
    3，Boolean();
    4，Object();
    5，Array();
    6，Function();
    7，Date();
    8，RegExp();
    9，Error()

- Object.create 创建
  - Object.create 创建对象是创建一个拥有指定原型和若干个指定属性的对象，即可任意指定原型，甚至是 null，Object.create（null）即没有原型；

### 3.原型链

- 理解：当读取实例对象的属性时，如果找不到，就会查找与对象关联的原型中的属性，如果还查不到，就去找原型的原型，一直找到最顶层<b style="color:red">（Object.prototype 是所有对象的原型链终点）</b>为止，于是就形成了一条原型链。
- 原型链是由一些用来继承和共享属性的对象组成的（有限的）对象链
- 图解 1：
  {% asset_img protoline1.jpg 原型链 %}
- 图解 2：
  {% asset_img protoline2.jpg 原型链 %}
- 注意事项：
  - <b style="color:pink">任何函数都是 Funcion 实例！Object 也是 Function 的实例，任何对象都是 Object 的实例！</b>
  - <b style="color:yellow;">Function.\_proto\_ === Function.prototype</b>
    - Function.prototype 是引擎创造出来的对象，一开始就有了，又因为其他的构造函数都可以通过原型链找到 Function.prototype，Function 本身也是一个构造函数，为了不产生混乱，就将这两个联系到一起了。
  - <b style="color:yellow;">Object.\_proto\_ === Function.prototype</b>
    - Object 是对象的构造函数，那么它也是一个函数，当然它的\_proto\_也是指向 Function.prototype

### 4.获取和设置原型

- 使用 <b style="color:pink;">Object.setPrototypeOf</b> 可设置对象的原型，下面的示例中继承关系为 obj>cw>home。
  <b style="color:pink;">Object.getPrototypeOf </b>用于获取一个对象的原型。

  ```js
  let obj = {
      name: "超威"
    };
    let cw = {
      web: "ChaoWei"
    };
    let home = {
      address: "RuiJin"
    };
    //让obj继承cw，即设置obj的原型为cw
    Object.setPrototypeOf(obj, cw);
    Object.setPrototypeOf(cw, home);
    console.log(obj);
    console.log(Object.getPrototypeOf(cw) == home); //true
  </script>
  ```

  - 打印图

    {% asset_img madeproto.png 设置原型 %}

### 5.原型检测

- 使用 <b style="color:pink">instanceof</b> 检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上

  ```js
  function A() {}
  function B() {}
  function C() {}

  const c = new C()
  B.prototype = c
  const b = new B()
  A.prototype = b
  const a = new A()

  console.dir(a instanceof A) //true
  console.dir(a instanceof B) //true
  console.dir(a instanceof C) //true
  console.dir(b instanceof C) //true
  console.dir(c instanceof B) //false
  ```

- 使用 <b style="color:pink">isPrototypeOf</b>检测一个对象是否是另一个对象的原型链中（即是否为另一个对象的长辈）

  ```js
  const a = {}
  const b = {}
  const c = {}

  Object.setPrototypeOf(a, b)
  Object.setPrototypeOf(b, c)

  console.log(b.isPrototypeOf(a)) //true
  console.log(c.isPrototypeOf(a)) //true
  console.log(c.isPrototypeOf(b)) //true
  ```

- 使用 <b style="color:pink">in 结合 hasOwnProperty </b>来判断属性是否在原型中
  - 对象设置属性，只是修改对象属性并不会修改原型属性，使用 <b style="color:pink">hasOwnProperty</b> 判断对象本身是否含有属性并不会检测原型。
  - 使用 in 判断某属性是否在对象中会检测原型与对象中，而 hasOwnProperty 只检测对象，所以结合后可判断属性是否在原型中，在使用 in 遍历的时候要注意 in 会攀升到原型链里，如果不让其遍历到原型链的话，需要加一个 if 使用 hasOwnProperty 判断一下；
  ```js
  function User() {}
  User.prototype.name = "超威"
  const cw = new User()
  //in会在原型中检测
  console.log("name" in cw)
  //hasOwnProperty 检测对象属性
  console.log(cw.hasOwnProperty("name"))
  ```

### 6.使用建议

- 建议使用 prototype 更改构造函数原型，使用 Object.setPrototypeOf 与 Object.getPrototypeOf 获取或设置原型。

## 继承

### 1.原型的继承

- 不是通过改变构造函数的原型实现的，这样会让多个构造函数共用一个原型，需要保留自己的原型；
- 继承是原型的继承，保留本身原型的方法，也可以继承别的原型对象上的方法；

```js
function User() {}
function Editor() {}
Editor.prototype.work = function () {}
//不要使用该方法继承:会让User的原型覆盖掉Editor的原型
//Editor.prototype = User.prototype
//1:可以将User.prototype赋值给Editor的原型对象的_proto_，即可实现Editor对User的原型继承,方法代码不限顺序；
Editor.prototype._proto_ = User.prototype
//2:可以使用Object.create()来实现继承，该方法需要注意代码顺序；
Editor.prototype = Object.create(User.prototype)
//这些原型对象最终都指向Object.prototype
```

- 继承对新增对象及 constructor 的影响

  - <b style="color:pink">Editor.prototype = Object.create(User.prototype)</b>
    - 该方式新建的实例对象不能继承 Editor 本身 的 prototype，只能继承 User 的 prototype；
    - 而且 本身的原型对象的 constructor 会丢失；需要加上 Editor.prototype.constructor = Editor ；
    - 注意通过上述加上的 constructor 默认会被 in 遍历出来；
    ```js
    //Object.defineProperty定义来禁止遍历constructor属性
    Object.defineProperty(Admin.prototype, "constructor", {
      value: Admin,
      enumerable: false, //禁止遍历
    })
    ```
  - <b style="color:pink">Editor.prototype._proto_ = User.prototype</b>
    > 该方式都不会出现上述问题

### 2.父类构造函数完成对象的属性初始化

```js
function User(name, age) {
  this.name = name
  this.age = age
}
User.prototype.getMessage = function () {
  console.log(this.name, this.age)
}
//使用call更改User里面的this指向
// function Editor(name, age){
//   User.call(this, name, age)
// }
//使用apply更改User里面的this指向
function Editor(...args) {
  User.apply(this, args)
}
Editor.prototype = Object.create(User.prototype)
Object.defineProperty(Editor.prototype, "constructor", {
  value: Editor,
  enumerable: false, //禁止遍历
})
// console.dir(Editor)
let cw = new Editor("超威", "21")
cw.getMessage() //超威 21

function Worker(...args) {
  User.apply(this, args)
}
Worker.prototype = Object.create(User.prototype)
Object.defineProperty(Worker.prototype, "constructor", {
  value: Worker,
  enumerable: false, //禁止遍历
})
let lm = new Worker("蓝猫", "18")
lm.getMessage() //蓝猫 18
```

### 3.使用原型工厂封装继承

```js
//上述重复的代码封装一下
function extend(son, father) {
  son.prototype = Object.create(father.prototype)
  Object.defineProperty(son.prototype, "constructor", {
    value: son,
    enumerable: false, //禁止遍历
  })
}
extend(Editor, User)
extend(Worker, User)
```

### 4.使用对象工厂派生对象并实现继承

```js
function User(name, age) {
  this.name = name
  this.age = age
}
User.prototype.getMessage = function () {
  console.log(this.name, this.age)
}
//在函数中创建对象
function editor(name, age) {
  const instance = Object.create(User.prototype)
  User.call(instance, name, age)
  instance.show = function () {
    console.log("show")
  }
  return instance
}
let cw = editor("超威", 21)
cw.getMessage() //超威 21
cw.show() //show
```

### <b style="color:pink">5.多继承</b>

- JS 无法实现多继承，即原型无法指向多个互不相干的对象，传统只能原型先指向对象 1，然后对象 1 的原型指向对象 2 ...这样只能层层继承，会造成比较混乱的现象；
- 使用 mixin （混合功能）实现多继承：先写好一些类，这些类为其他的类提供服务（不是继承）；即先定义一些功能的对象，将其压入原型对象里即可；
  - Object.assign 方法用来将源对象（source）的所有可枚举属性，复制到目标对象（target）。它至少需要两个对象作为参数，第一个参数是目标对象，后面的参数都是源对象。

```js
let eat = {
  eating() {
    console.log("eating")
  },
}
let sleep = {
  sleeping() {
    console.log("sleeping")
  },
}
function extend(son, father) {
  son.prototype = Object.create(father.prototype)
  son.prototype.constructor = son
}
function User(name, age) {
  this.name = name
  this.age = age
}
User.prototype.getMessage = function () {
  console.log(this.name, this.age)
}
function People(name, age) {
  User.call(this, name, age)
}
//先让People继承User
extend(People, User)
//再通过Object.assign将eat对象和sleep对象统一压入People.prototype对象中
Object.assign(People.prototype, eat, sleep)
let cw = new People("超威", 21)
cw.getMessage() //超威 21
cw.eating() //eating
cw.sleeping() //sleeping
```

### 6.使用 super 实现 mixin 内部继承

- <a href="https://www.cnblogs.com/pwindy/p/12666846.html" target="_blank">super 详细</a>

```js
let eat = {
  eating() {
    return "eating"
  },
}
let sleep = {
  sleeping() {
    console.log("sleeping")
  },
}
let play = {
  __proto__: eat,
  playing() {
    //super表示当前对象的原型对象
    console.log(super.eating() + "，then playing")
  },
}
function extend(son, father) {
  son.prototype = Object.create(father.prototype)
  son.prototype.constructor = son
}
function User(name, age) {
  this.name = name
  this.age = age
}
User.prototype.getMessage = function () {
  console.log(this.name, this.age)
}
function People(name, age) {
  User.call(this, name, age)
}
//将eating和sleeping统一压入People.prototype中
extend(People, User)
Object.assign(People.prototype, eat, sleep, play)
let cw = new People("超威", 21)
cw.getMessage() //超威 21
console.log(cw.eating()) //eating
cw.sleeping() //sleeping
cw.playing() //eating，then playing
```
