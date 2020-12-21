---
title: JS数组方法
date: 2020/12/20
tags: [前端面试, JS数组]
categories: Javascript
---

## JS 数组方法

### <b style="color:skyblue">1.修改器方法（9）</b>

- <b style="color:#e33946">会改变调用的原对象数组本身的方法</b>

- <b style="color:pink">_1.push_</b>
  > 语法：arr.push(element1, ..., elementN)
  - 作用：将一个或多个元素添加到数组的末尾，并返回该数组的新长度。
  - 返回值：增加数组后的长度；
- <b style="color:pink">_2.pop_</b>
  > 语法：arr.pop()
  - 作用：删除数组中最后一个元素，并返回该元素的值；
  - 返回值：删除的项目，删除为空时返回 undefined；
- <b style="color:pink">_3.shift_</b>
  > 语法：arr.shift()
  - 作用：从数组中删除第一个元素，并返回该元素的值。
  - 返回值：删除的元素；
- <b style="color:pink">_4.unshift_</b>

  > 语法：arr.unshift(element1, ..., elementN)

  - 作用：将一个或多个元素添加到数组的开头，并返回该数组的新长度。
  - 返回值：增加数组后的新长度；

- <b style="color:pink">_5.sort_</b>
  > 语法：arr.sort([compareFunction])
  - 作用：用原地算法对数组的元素进行排序，并返回数组。默认排序顺序(未指定排序 compareFunction)是在将元素转换为字符串，然后比较它们的 UTF-16 代码单元值序列时构建的；即'80' < '9';
  - 返回值：排列之后的数组
  - 比较函数格式
  ```js
  function compare(a, b) {
    if (a < b) {
      // 按某种排序标准进行比较, a 小于 b，升序排列
      return -1
    }
    if (a > b) {
      //降序排列
      return 1
    }
    // a must be equal to b
    return 0
  }
  ```
- <b style="color:pink">_6.reserve_</b>
  > 语法：arr.reverse()
  - 作用：数组中元素的位置颠倒，并返回该数组。数组的第一个元素会变成最后一个，数组的最后一个元素变成第一个。
  - 返回值：倒序排列之后的数组。
- <b style="color:pink">_7.splice_</b>
  > 语法：array.splice(start[, deleteCount[, item1[, item2[, ...]]]])
  - 参数：start：开始时的索引（可为负数）；deleteCount：从开始处要删除的元素个数；item1...：从开始处新增的元素；
  - 作用：通过删除或替换现有元素或者原地添加新的元素来修改数组,并以数组形式返回被修改的内容。
  - 返回值：由被删除的元素组成的一个数组。如果只删除了一个元素，则返回只包含一个元素的数组。如果没有删除元素，则返回空数组。
- <b style="color:pink">8.fill</b>
  > 语法：arr.fill(value[, start[, end]])
  - 参数：value：用来填充数组元素的值；start：其实索引，默认值为 0；end：终止索引：默认值为 this.length;
  - 作用：用一个固定值填充一个数组中从起始索引到终止索引内的全部元素。不包括终止索引。
  - 返回值：修改后的数组。
- <b style="color:pink">9.copyWithin</b>

  > 语法：arr.copyWithin(target[, start[, end]])

  - 参数：target：复制后存放处；start：开始复制的索引；end：结束复制的索引，不包括 end 该位置的元素；
  - 作用：浅复制数组的一部分到同一数组中的另一个位置，并返回它，不会改变原数组的长度。
  - 返回值：改变后的数组。
  - 例子：
    ```js
    const array1 = ["a", "b", "c","d", "e"]
    console.log(array1.copyWithin(0,3, 4)) //Array ["d", "b", "c","d", "e"]
    console.log(array1.copyWithin(1, 3);
     // Array ["d","d", "e", "d", "e"]
    ```

### <b style="color:skyblue">2.访问方法</b>

- <b style="color:#e33946">下面数组方法不会改变调用原数组的值，只会返回一个新的数组或者返回一个其它的期望值。</b>
- <b style="color:#17c37b">_1.concat_</b>

  > 语法：let new_array = old_array.concat(value1[, value2[, ...[, valueN]]])

  - 作用：用于合并两个或多个数组。此方法不会更改现有数组，而是返回一个新数组。

  - 返回值：新的数组实例。

- <b style="color:#17c37b">_2.includes_</b>
  > 语法：arr.includes(valueToFind[, fromIndex])
  - 参数：fromIndex：开始查找的索引，可以为负数。
  - 作用：用来判断一个数组是否包含一个指定的值(比较字符串和字符时是区分大小写)，根据情况，如果包含则返回 true，否则返回 false。
  - 返回值：返回一个布尔值。
- <b style="color:#17c37b">_3.join_</b>
  > 语法：arr.join([separator])
  - 作用：将一个数组（或一个类数组对象）的所有元素连接成一个字符串并返回这个字符串。如果数组只有一个项目，那么将返回该项目而不使用分隔符，如果一个元素为 undefined 或 null，它会被转换为空字符串。
  - 返回值：一个所有数组元素连接的字符串。如果 arr.length 为 0，则返回空字符串。
- <b style="color:#17c37b">_4.slice_</b>
  > 语法：arr.slice([begin[, end]])
  - 作用：返回一个新的数组对象，这一对象是一个由 begin 和 end 决定的原数组的浅拷贝（包括 begin，不包括 end）。原始数组不会被改变。
  - 返回值：一个含有被提取元素的新数组。
- <b style="color:#17c37b">_5.toString_</b>

  > 语法：arr.toString()

  - 作用：返回一个字符串，表示指定的数组及其元素。
  - 返回值：一个表示指定的数组及其元素的字符串。

- <b style="color:#17c37b">_6.indexOf_</b>
  > 语法：arr.indexOf(searchElement[, fromIndex])
  - 作用：返回在数组中可以找到一个给定元素的第一个索引，如果不存在，则返回-1。
  - 返回值：首个被找到的元素在数组中的索引位置; 若没有找到则返回 -1
- <b style="color:#17c37b">_7.lastIndexOf_</b>

  > 语法：arr.lastIndexOf(searchElement[, fromIndex])

  - 作用：返回指定元素（也即有效的 JavaScript 值或变量）在数组中的最后一个的索引，如果不存在则返回 -1。从数组的后面向前查找，从 fromIndex 处开始。
  - 返回值：数组中该元素最后一次出现的索引，如未找到返回-1。

### <b style="color:skyblue">3.迭代方法</b>

- 在下面的众多遍历方法中，有很多方法都需要指定一个<b style="color:red">回调函数</b>作为参数。
- <b style="color:#864bff">_1.forEach_</b>
  > 语法：arr.forEach(callback(currentValue [, index [, array]])[, thisArg])
  - 参数：
    - currentValue：数组中正在处理的当前元素；
    - index：数组中正在处理的当前元素的索引；
    - array ：forEach() 方法正在操作的数组；
    - thisArg ：当执行回调函数 callback 时作 this 的值。
  - 作用：对数组的每个元素执行一次给定的函数。
  - 返回值：undefined
- <b style="color:#864bff">_2.filter_</b>
  > 语法：let newArray = arr.filter(callback(element[, index[, array]])[, thisArg])
  - 作用：创建一个新数组, 其包含通过所提供函数实现的测试的所有元素。
  - 返回值：一个新的、由通过测试的元素组成的数组，如果没有任何数组元素通过测试，则返回空数组。
- <b style="color:#864bff">_3.map_</b>

  > 语法：

  ```js
  let new_array = arr.map(function callback(currentValue[, index[, array]]) {
  // Return element for new_array
  }[, thisArg])
  //通常callback函数只需要接收一个参数
  ```

  - 作用：创建一个新数组，其结果是该数组中的每个元素是调用一次提供的函数后的返回值。
  - 返回值：一个由原数组每个元素执行回调函数的结果组成的新数组。
  - 使用场景：使用 map 重新格式化数组中的对象

- <b style="color:#864bff">_4.reduce_</b>

  > 语法：arr.reduce(callback(accumulator, currentValue[, index[, array]])[, initialValue])

  - 参数：
    - accumulator：累计器累计回调的返回值; 它是上一次调用回调时返回的累积值，或 initialValue；
    - currentValue：数组中正在处理的元素；
    - initialValue：作为第一次调用 callback 函数时的第一个参数的值。 如果没有提供初始值，则将使用数组中的第一个元素。 在没有初始值的空数组上调用 reduce 将报错。
  - 作用：对数组中的每个元素执行一个由您提供的 reducer 函数(升序执行)，将其结果汇总为单个返回值。
  - 返回值：函数累计处理的结果
  - 应用场景：

    - 计算总合

    ```js
    const array1 = [1, 2, 3, 4]
    const reducer = (accumulator,currentValue) => accumulator +currentValue
    //1 + 2 + 3 + 4
    console.log(array1.reduce(reducer) //10
    // 5 + 1 + 2 + 3 + 4
    console.log(array1.reduce(reducer,5)) // 15
    ```

    - 简单数组去重

    ```js
    let arr = [1,2,3,4,5,2,3]
    let result = arr.reduce((prev,cur=>{
    if (!prev.includes(cur)){
    prev.push(cur)
    }
    return prev
    },[])
    console.log(result)
    ```

    - 计算数组中的元素在数组中的出现次数

    ```js
    let arr = [1, 2, 3, 4, 5, 2, 3]
    let result = arr.reduce((prev, cur) => {
      if (prev[cur] != undefined) {
        prev[cur]++
      } else {
        prev[cur] = 1
      }
      return prev
    }, {})
    console.log(result)
    ```

## forEach、map、filter、reduce 的区别

- 相同点：
  - 都会循环遍历数组中的每一项；
  - map()、forEach()和 filter()方法里每次执行匿名函数都支持 3 个参数，参数分别是：当前元素、当前元素的索引、当前元素所属的数组；
  - 匿名函数中的 this 都是指向 window；
  - 只能遍历数组。
- 不同点：
  - map()速度比 forEach()快；
  - map()和 filter()会返回一个新数组，不对原数组产生影响；
  - forEach()不会产生新数组，返回 undefined；
  - reduce()函数是把数组缩减为一个值(比如求和、求积)；
  - reduce()有 4 个参数，第一个参数为初始值。

## 常见数组方法应用场景

### <b style="color:#005397">1.数组去重</b>

- 1：使用 splice 和 for 循环嵌套方法去重（ES5 最常用）

```js
let arr = [2, 1, 3, 4, 3, 5, 6, 6, 4, 3, 8]
for (let i = 0; i < arr.length; i++) {
  for (let j = i + 1; j < arr.length; j++) {
    if (arr[i] == arr[j]) {
      arr.splice(j, 1)
      j--
    }
  }
}
console.log(arr) //[2, 1, 3, 4, 5, 6, 8]
```

- 2：使用 indexof 方法去重

```js
let arr = [2, 1, 3, 4, 3, 5, 6, 6, 4, 3, 8]
let newArr = []
for (let i = 0; i < arr.length; i++) {
  if (newArr.indexOf(arr[i]) === -1) {
    newArr.push(arr[i])
  }
}
console.log(arr) //[2, 1, 3, 4, 3, 5, 6, 6, 4, 3, 8]
console.log(newArr) //[2, 1, 3, 4, 5, 6, 8]
```

- 3：使用 includes 方法去重

```js
let newArr = []
for (let i = 0; i < arr.length; i++) {
  if (!newArr.includes(arr[i])) {
    newArr.push(arr[i])
  }
}
console.log(newArr) //[2, 1, 3, 4, 5, 6, 8]
```

- 4：使用 reduce 方法去重

```js
let newArr = arr.reduce(function (itemlater, currentValue) {
  //注意初始需要传入空数组作为itemlater
  if (itemlater.indexOf(currentValue) == -1) {
    itemlater.push(currentValue)
  }
  return itemlater
}, [])
console.log(arr) //[2, 1, 3, 4, 3, 5, 6, 6, 4, 3, 8]
console.log(newArr) //[2, 1, 3, 4, 5, 6, 8]
```

- 5：使用 Set 去重（ES6 最常用）

```js
function unique(arr) {
  //Set数据结构，它类似于数组，其成员的值都是唯一的
  return Array.from(new Set(arr)) // 利用Array.from将Set结构转换成数组
}
console.log(unique(arr)) //[2, 1, 3, 4, 5, 6, 8]
```

---

参考文献<br>

- [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)
- [红鲤鱼与 LV 博客园](https://www.cnblogs.com/kaiqinzhang/p/11496151.html)
