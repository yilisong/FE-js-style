# es6中常用知识

* [1 箭头函数](#1-箭头函数)
  

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

  参考：https://imququ.com/post/arrow-function-in-es6.html

```