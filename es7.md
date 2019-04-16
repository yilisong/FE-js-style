# es7中常用知识

* [1 includes](#1-includes)
* [2 指数操作符](#2-指数操作符)

## 1 includes

```javascript
ES2016添加了两个小的特性来说明标准化过程：

数组includes()方法，用来判断一个数组是否包含一个指定的值，根据情况，如果包含则返回true，否则返回false。
a ** b指数运算符，它与 Math.pow(a, b)相同。
1.Array.prototype.includes()
includes() 函数用来判断一个数组是否包含一个指定的值，如果包含则返回 true，否则返回false。

includes 函数与 indexOf 函数很相似，下面两个表达式是等价的：

arr.includes(x)
  arr.indexOf(x) >= 0

接下来我们来判断数字中是否包含某个元素：

在ES7之前的做法

使用indexOf()验证数组中是否存在某个元素，这时需要根据返回值是否为-1来判断：

let arr = ['react', 'angular', 'vue']

if (arr.indexOf('react') !== -1) {
  console.log('react存在')
}

使用ES7的includes()

使用includes()验证数组中是否存在某个元素，这样更加直观简单：

let arr = ['react', 'angular', 'vue']

if (arr.includes('react')) {
  console.log('react存在')
}
```

## 2 指数操作符

```javascript
在ES7中引入了指数运算符**，**具有与Math.pow(..)等效的计算结果。

不使用指数操作符

使用自定义的递归函数calculateExponent或者Math.pow()进行指数运算：

function calculateExponent(base, exponent) {
  if (exponent === 1) {
    return base
  } else {
    return base * calculateExponent(base, exponent - 1)
  }
}

  console.log(calculateExponent(2, 10)) // 输出1024
  console.log(Math.pow(2, 10)) // 输出1024
使用指数操作符

使用指数运算符**，就像+、-等操作符一样：

console.log(2**10)// 输出1024
  
```
