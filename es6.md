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
    console.log("当前时间:", t)  // 引用错误（ReferenceError）
    ...
    let t = readTachymeter()
  }

  let t = readTachymeter()  // 正确

  for (let i = 0; i < messages.length; i++) {  // 正确
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
      this.name = name;
      this.color = color;
    }
    // toString 是原型对象上的属性
    toString() {
      console.log('name:' + this.name + ',color:' + this.color);
    }
  }

  var animal = new Animal('dog','white');//实例化Animal
  animal.toString();

  console.log(animal.hasOwnProperty('name')); //true
  console.log(animal.hasOwnProperty('toString')); // false
  console.log(animal.__proto__.hasOwnProperty('toString')); // true

  class Cat extends Animal {
    constructor(action) {
    // 子类必须要在constructor中指定super 函数，否则在新建实例的时候会报错.
    // 如果没有置顶consructor,默认带super函数的constructor将会被添加、
      super('cat','white');
      this.action = action;
    }
    toString() {
      console.log(super.toString());
    }
  }

  var cat = new Cat('catch')
  cat.toString();

  // 实例cat 是 Cat 和 Animal 的实例，和Es5完全一致。
  console.log(cat instanceof Cat); // true
  console.log(cat instanceof Animal); // true

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

心得：ES6不仅支持变量的导出，也支持常量的导出。 export const sqrt = Math.sqrt;//导出常量

ES6将一个文件视为一个模块，上面的模块通过 export 向外输出了一个变量。一个模块也可以同时往外面输出多个变量。

 //test.js
  var name = 'Rainbow';
  var age = '24';
  export {name, age};

导出函数

// myModule.js
  export function myModule(someArg) {
  return someArg;
  }  

导入(import)
定义好模块的输出以后就可以在另外一个模块通过import引用。

import {myModule} from 'myModule';// main.js
  import {name,age} from 'test';// test.js

心得:一条import 语句可以同时导入默认函数和其它变量。import defaultMethod, { otherMethod } from 'xxx.js';
```
<!-- * [5 函数参数默认值](#5-函数参数默认值)  
* [6 模板字符串](#6-模板字符串)  
* [7 解构赋值](#7-解构赋值)  
* [8 延展操作符](#8-延展操作符)  
* [9 对象属性简写](#9-对象属性简写)  
* [10 Promise](#10-Promise) -->