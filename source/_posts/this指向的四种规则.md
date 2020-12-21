---
title: this指向的四种规则
date: 2020/12/19
tags: [前端面试, this]
categories: Javascript
---

## this 的指向的四种规则

### <b style="color:pink">1.默认绑定规则</b>

- this 在全局环境中默认指向 window
  ```js
  console.log(this === window) //true
  ```
- 函数的独立调用 this 也是默认指向 window
  - 常见场景
    ```js
    function a() {
      console.log(this)
    }
    a() //window
    ```
  - 立即执行函数中的 this 也是默认指向 window
    ```js
    ;(function () {
      console.log(this)
    })() //window
    ```
  - 涉及闭包的情况
    ```js
    let obj = {
      a: 2,
      foo: function () {
        console.log(this) //指向obj对象
        function test() {
          console.log(this) //指向window
        }
        return test
      },
    }
    obj.foo()() //此时obj.foo()相当于test，故 仍是独立调用
    ```

### <b style="color:pink">2.隐式绑定规则</b>

- 通过对象的属性（方法）来调用，谁调用就指向谁
- 每一个函数执行都会有自身的 this 指向，由当前的执行方式决定的；
- 作为一个 DOM 事件处理函数：当函数被用作事件处理函数时，它的 this 指向触发事件的元素
- 常见场景

  ```js
  let obj = {
    a: 2,
    foo() {
      console.log(this) //指向obj对象
      function test() {
        //此时test()相当于独立调用
        console.log(this) //指向window
      }
      test()
    },
  }
  obj.foo()
  ```

- 变量赋值的情况
  ```js
  let obj = {
    a: 2,
    foo() {
      console.log(this) //指向window
      function test() {
        //此时test()相当于独立调用
        console.log(this) //指向window
      }
      test()
    },
  }
  //此时未执行obj的foo方法，只是将其引用赋值给bar，之后bar()调用就相当于独立调用；
  let bar = obj.foo
  bar()
  ```
- 参数赋值的情况
  ```js
  let obj = {
    a: 2,
    foo: function () {
      console.log(this)
    },
  }
  function bar(fn) {
    //父函数bar是有能力决定子函数fn的this指向
    fn()
  }
  //预编译的过程中，实参被赋值为形参（浅拷贝）
  bar(obj.foo) //window
  ```
- 高阶函数（参数有函数的）
  - 父函数是有能力决定子函数（回调函数）的 this 指向的，需要查阅这些高阶函数的 api 文档查看 this；
  - 比如 arr.forEach(function(){},this 指向对象)，setinterval(function(){})等等；

### <b style="color:pink">3.显示绑定规则</b>

- apply
  - apply 接受两个参数，第一个参数是 this 的指向，第二个参数是函数接受的参数，以数组的形式传入，且当第一个参数为 null、undefined 的时候，默认指向 window(在浏览器中)，使用 apply 方法改变 this 指向后原函数会立即执行，且此方法只是临时改变 thi 指向一次。
  ```js
  var arr = [1, 10, 5, 8, 3]
  console.log(Math.max.apply(null, arr)) //10
  ```
- call
  - call 方法的第一个参数也是 this 的指向，后面传入的是一个参数列表（注意和 apply 传参的区别）。当一个参数为 null 或 undefined 的时候，表示指向 window（在浏览器中），和 apply 一样，call 也只是临时改变一次 this 指向，并立即执行。
  ```js
  var arr = [1, 10, 5, 8, 3]
  console.log(Math.max.call(null, arr[0], arr[1], arr[2], arr[3], arr[4])) //10
  ```
- bind
  - bind 方法和 call 很相似，第一参数也是 this 的指向，后面传入的也是一个参数列表(但是这个参数列表可以分多次传入，call 则必须一次性传入所有参数)，但是它改变 this 指向后不会立即执行，而是返回一个永久改变 this 指向的函数。
  ```js
  var arr = [1, 10, 5, 8, 12]
  var max = Math.max.bind(null, arr[0], arr[1], arr[2], arr[3])
  console.log(max(arr[4])) //12，分两次传参最后函数运行时会把所有参数连接起来一起放入函数运行。
  ```
- 三者的区别
  - 三者都可以改变函数的 this 对象指向。
  - 三者第一个参数都是 this 要指向的对象，如果如果没有这个参数或参数为 undefined 或 null，则默认指向全局 window。
  - 三者都可以传参，但是 apply 是数组，而 call 是参数列表，且 apply 和 call 是一次性传入参数，而 bind 可以分为多次传入。
    bind 是返回绑定 this 之后的函数，便于稍后调用；apply 、call 则是立即执行 。

### <b style="color:pink">4.new 绑定规则</b>

- 当一个函数用作构造函数时（使用 new 关键字），它的 this 被绑定到正在构造的新对象。
  > 虽然构造函数返回的默认值是 this 所指的那个对象，但它仍可以手动返回其他的对象（如果返回值不是一个对象，则仍返回 this 对象）。
- 例子
  ```js
  function C() {
    //根据需要在this上创建属性，然后赋值给它们
    this.a = 37
  }
  var o = new C()
  console.log(o.a) //37
  function C2() {
    this.a = 37
    //手动的设置了返回对象，与this绑定的默认对象被丢弃了
    return { a: 38 }
  }
  o = new C2()
  console.log(o.a) //38
  ```

### <b style="color:pink">5. 4 种绑定规则 this 的优先级</b>

- new > 显示 > 隐式 > 默认

### <b style="color:pink">6.箭头函数 this 相关</b>

- 箭头函数内部没有 this 指向，this 是由外层函数（父函数）作用域中的 this 决定的，无论通过上者四种绑定规则都对 this 无效，即箭头函数的 this 被永久绑定到了它外层函数的 this；
- 通过对象的方法来隐式绑定箭头函数

  ```js
  function foo(){
    console.log(this)//obj
    let test = () => {
      console.log(this)//obj
    }
    test()

  let obj = {
    a: 1,
    foo: foo
  }
  //通过对象的方法来隐式调用箭头函数
  obj.foo()
  ```

- 通过独立调用(默认绑定规则)对箭头函数无效，即 this 不一定会指向 window

  ```js
  function foo() {
    console.log(this) //obj
    let test = () => {
      console.log(this) //obj
    }
    return test
  }

  let obj = {
    a: 1,
    foo: foo,
  }
  //通过箭头函数的独立调用
  obj.foo()()
  ```

- 显示绑定规则对箭头函数也无效

  ```js
  function foo() {
    console.log(this) //window 独立调用foo，this指向window
    let test = () => {
      console.log(this) //window 显示调用改变了this指向，但对箭头函数无效，仍然跟外层this一致指向window
    }
    return test
  }

  let obj = {
    a: 1,
    foo: foo,
  }
  let bar = foo().call(obj)
  ```

- new 不能实例箭头函数，会报错

  ```js
  let foo = () => {
    console.log(this)
  }
  new foo() //报错
  ```

---

<i style="font-size:13px">参考文献：<br>[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/this)<br>[前端小夏老师](https://www.bilibili.com/video/BV1NT4y1j7xH?from=search&seid=11721105465063872258)</i>
