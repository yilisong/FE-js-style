# es6中常用知识

* [1 箭头函数](#1-箭头函数)
* [2 箭头函数](#2-let和const区别)  

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