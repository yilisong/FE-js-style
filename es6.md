# es6中常用知识

* [1 箭头函数](#1-箭头函数)
* [2 let和const区别](#2-let和const区别)  
* [3 类](#3-类)  
* [4 模块化](#4-模块化)  
* [5 函数参数默认值](#5-函数参数默认值)  
* [6 模板字符串](#6-模板字符串)  
* [7 解构赋值](#7-解构赋值)  
* [8 延展操作符](#8-延展操作符)  
* [9 对象属性简写](#9-对象属性简写)  
* [10 Promise](#10-Promise)

## 1 箭头函数

```javascript
  ([param] [, param]) => {
    statements
  }

  param => expression

 // param 是参数，根据参数个数不同，分这几种情况：

  () => { ... } // 零个参数用 () 表示
  x => { ... } // 一个参数可以省略 ()
  (x, y) => { ... } // 多参数不能省略 ()

  参考：[arrow-function-in-es6]( https://imququ.com/post/arrow-function-in-es6.html)


```
## 2 let和const区别

```javascript
  以前我们习惯性的都用var去申明，而js本身是没有块级作用域这个说法的，
  后期为了方便我们开发，ES官方出了let和const
  
  //const
  const MAX_CAT_SIZE_KG = 3000 // 正确

  MAX_CAT_SIZE_KG = 5000 // 语法错误（SyntaxError）
  MAX_CAT_SIZE_KG++ // 虽然换了一种方式，但仍然会导致语法错误

  const theFairest  // 依然是语法错误

  //let
  function update() {
    console.log('当前时间:', t)  // 引用错误（ReferenceError）
    ...
    let t = readTachymeter()
  }

  let t = readTachymeter()  // 正确

  for (let i = 0 i < messages.length i++) {  // 正确
    ...
  }

  简单的来说这两个的差异就是 
    const申明的时候必须要赋值，并且不可更改
    let声明的变量拥有块级作用域
    let声明的全局变量不是全局对象的属性
    let声明的变量直到控制流到达该变量被定义的代码行时才会被装载，所以在到达之前使用该变量会触发错误
    用let重定义变量会抛出一个语法错误（SyntaxError）

```

## 3 类

```javascript
  对熟悉Java，object-c，c#等纯面向对象语言的开发者来说，都会对class有一种特殊的情怀。
  ES6 引入了class（类），让JavaScript的面向对象编程变得更加简单和易于理解。

  class Animal {
    // 构造函数，实例化的时候将会被调用，如果不指定，那么会有一个不带参数的默认构造函数.
    constructor(name,color) {
      this.name = name
      this.color = color
    }
    // toString 是原型对象上的属性
    toString() {
      console.log('name:' + this.name + ',color:' + this.color)
    }
  }

  var animal = new Animal('dog','white')//实例化Animal
  animal.toString()

  console.log(animal.hasOwnProperty('name')) //true
  console.log(animal.hasOwnProperty('toString')) // false
  console.log(animal.__proto__.hasOwnProperty('toString')) // true

  class Cat extends Animal {
    constructor(action) {
    // 子类必须要在constructor中指定super 函数，否则在新建实例的时候会报错.
    // 如果没有置顶consructor,默认带super函数的constructor将会被添加、
      super('cat','white')
      this.action = action
    }
    toString() {
      console.log(super.toString())
    }
  }

  var cat = new Cat('catch')
  cat.toString()

  // 实例cat 是 Cat 和 Animal 的实例，和Es5完全一致。
  console.log(cat instanceof Cat) // true
  console.log(cat instanceof Animal) // true

```

## 4 模块化

```javascript
ES5不支持原生的模块化，在ES6中模块作为重要的组成部分被添加进来。模块的功能主要由 export 和 import 组成。
每一个模块都有自己单独的作用域，模块之间的相互调用关系是通过 export 来规定模块对外暴露的接口，
通过import来引用其它模块提供的接口。同时还为模块创造了命名空间，防止函数的命名冲突。

导出(export)
ES6允许在一个模块中使用export来导出多个变量或函数。

导出变量
  //test.js
  export var name = 'Rainbow'

心得：ES6不仅支持变量的导出，也支持常量的导出。 export const sqrt = Math.sqrt//导出常量

ES6将一个文件视为一个模块，上面的模块通过 export 向外输出了一个变量。一个模块也可以同时往外面输出多个变量。

 //test.js
  var name = 'Rainbow'
  var age = '24'
  export {name, age}

导出函数

// myModule.js
  export function myModule(someArg) {
  return someArg
  }  

导入(import)
定义好模块的输出以后就可以在另外一个模块通过import引用。

import {myModule} from 'myModule'// main.js
  import {name,age} from 'test'// test.js

心得:一条import 语句可以同时导入默认函数和其它变量。import defaultMethod, { otherMethod } from 'xxx.js'
```

## 5 函数参数默认值
```javascript
ES6支持在定义函数的时候为其设置默认值：

function foo(height = 50, color = 'red') {
  // ...
}

不使用默认值：

function foo(height, color) {
  var height = height || 50
  var color = color || 'red'
  //...
}

这样写一般没问题，但当参数的布尔值为false时，就会有问题了。比如，我们这样调用foo函数：

foo(0, '')

因为0的布尔值为false，这样height的取值将是50。同理color的取值为‘red’。

所以说，函数参数默认值不仅能是代码变得更加简洁而且能规避一些问题。

```

## 6 模板字符串
```javascript
ES6支持模板字符串，使得字符串的拼接更加的简洁、直观。

不使用模板字符串：

var name = 'Your name is ' + first + ' ' + last + '.'

使用模板字符串：

var name = `Your name is ${first} ${last}.`

在ES6中通过${}就可以完成字符串的拼接，只需要将变量放在大括号之中。

```

## 7 解构赋值
```javascript
解构赋值语法是JavaScript的一种表达式，可以方便的从数组或者对象中快速提取值赋给定义的变量。

获取数组中的值
从数组中获取值并赋值到变量中，变量的顺序与数组中对象顺序对应。

var foo = ['one', 'two', 'three', 'four']

  var [one, two, three] = foo
  console.log(one) // 'one'
  console.log(two) // 'two'
  console.log(three) // 'three'

  //如果你要忽略某些值，你可以按照下面的写法获取你想要的值
  var [first, , , last] = foo
  console.log(first) // 'one'
  console.log(last) // 'four'

  //你也可以这样写
  var a, b //先声明变量

  [a, b] = [1, 2]
  console.log(a) // 1
  console.log(b) // 2

如果没有从数组中的获取到值，你可以为变量设置一个默认值。

var a, b

  [a=5, b=7] = [1]
  console.log(a) // 1
  console.log(b) // 7

通过解构赋值可以方便的交换两个变量的值。

var a = 1
  var b = 3

  [a, b] = [b, a]
  console.log(a) // 3
  console.log(b) // 1


获取对象中的值
const student = {
  name:'Ming',
  age:'18',
  city:'Shanghai'  
}

const {name,age,city} = student
console.log(name) // 'Ming'
console.log(age) // '18'
console.log(city) // 'Shanghai'

```

## 8 延展操作符
```javascript
延展操作符...可以在函数调用/数组构造时, 将数组表达式或者string在语法层面展开；还可以在构造对象时, 将对象表达式按key-value的方式展开。

语法
函数调用：

myFunction(...iterableObj)

数组构造或字符串：

[...iterableObj, '4', ...'hello', 6]

构造对象时,进行克隆或者属性拷贝（ECMAScript 2018规范新增特性）：

let objClone = { ...obj }

应用场景
在函数调用时使用延展操作符

function sum(x, y, z) {
  return x + y + z
  }
  const numbers = [1, 2, 3]

  //不使用延展操作符
  console.log(sum.apply(null, numbers))

  //使用延展操作符
  console.log(sum(...numbers))// 6

构造数组

没有展开语法的时候，只能组合使用 push，splice，concat 等方法，来将已有数组元素变成新数组的一部分。有了展开语法, 构造新数组会变得更简单、更优雅：

const stuendts = ['Jine','Tom'] 
  const persons = ['Tony',... stuendts,'Aaron','Anna']
  conslog.log(persions)// ["Tony", "Jine", "Tom", "Aaron", "Anna"]

和参数列表的展开类似, ... 在构造字数组时, 可以在任意位置多次使用。

数组拷贝

var arr = [1, 2, 3]
  var arr2 = [...arr] // 等同于 arr.slice()
  arr2.push(4) 
  console.log(arr2)//[1, 2, 3, 4]

展开语法和 Object.assign() 行为一致, 执行的都是浅拷贝(只遍历一层)。

连接多个数组

var arr1 = [0, 1, 2]
  var arr2 = [3, 4, 5]
  var arr3 = [...arr1, ...arr2]// 将 arr2 中所有元素附加到 arr1 后面并返回
  //等同于
  var arr4 = arr1.concat(arr2)

在ECMAScript 2018中延展操作符增加了对对象的支持
var obj1 = { foo: 'bar', x: 42 }
  var obj2 = { foo: 'baz', y: 13 }

  var clonedObj = { ...obj1 }
  // 克隆后的对象: { foo: "bar", x: 42 }

  var mergedObj = { ...obj1, ...obj2 }
  // 合并后的对象: { foo: "baz", x: 42, y: 13 }

  ```javascript

  #### 在React中的应用

  通常我们在封装一个组件时，会对外公开一些 props 用于实现功能。大部分情况下在外部使用都应显示的传递 props 。但是当传递大量的props时，会非常繁琐，这时我们可以使用 `...(延展操作符,用于取出参数对象的所有可遍历属性)` 来进行传递。

  #### 一般情况下我们应该这样写

  ```javascript
  <CustomComponent name ='Jine' age ={21} />

使用 ... ，等同于上面的写法

const params = {
  name: 'Jine',
  age: 21
  }
  <CustomComponent {...params} />
配合解构赋值避免传入一些不需要的参数

var params = {
  name: '123',
  title: '456',
  type: 'aaa'
  }

  var { type, ...other } = params

  <CustomComponent type='normal' number={2} {...other} />
  //等同于
  <CustomComponent type='normal' number={2} name='123' title='456' />

  

```

## 9 对象属性简写
```javascript
在ES6中允许我们在设置一个对象的属性的时候不指定属性名。

不使用ES6

  const name='Ming',age='18',city='Shanghai'

  const student = {
    name:name,
    age:age,
    city:city
  }
  console.log(student)//{name: "Ming", age: "18", city: "Shanghai"}

对象中必须包含属性和值，显得非常冗余。

使用ES6

  const name='Ming',age='18',city='Shanghai'

  const student = {
    name,
    age,
    city
  }
  console.log(student)//{name: "Ming", age: "18", city: "Shanghai"}

对象中直接写变量，非常简洁。
```

## 10 Promise
```javascript
Promise 是异步编程的一种解决方案，比传统的解决方案callback更加的优雅。
它最早由社区提出和实现的，ES6 将其写进了语言标准，统一了用法，原生提供了Promise对象。

不使用ES6
嵌套两个setTimeout回调函数：

setTimeout(function() {
  console.log('Hello') // 1秒后输出"Hello"
  setTimeout(function() {
    console.log('Hi') // 2秒后输出"Hi"
  }, 1000)
}, 1000)

使用ES6

var waitSecond = new Promise(function(resolve, reject) {
  setTimeout(resolve, 1000)
})

waitSecond .then(function() {
  console.log("Hello") // 1秒后输出"Hello"
  return waitSecond
}).then(function() {
  console.log("Hi") // 2秒后输出"Hi"
})

上面的的代码使用两个then来进行异步编程串行化，避免了回调地狱：
```
