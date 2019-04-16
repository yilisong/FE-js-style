# es9中常用知识

* [1 异步迭代](#1-异步迭代)
* [2 Promise.finally()](#2-Promise.finally())
* [3 Rest/Spread属性](#3-Rest/Spread属性)
* [4 正则表达式命名捕获组](#4-正则表达式命名捕获组)
* [5 正则表达式反向断言(lookbehind)](#5-正则表达式反向断言(lookbehind))
* [6 正则表达式dotAll模式](#6-正则表达式dotAll模式)
* [7 正则表达式Unicode转义](#7-正则表达式Unicode转义)
* [8 非转义序列的模板字符串](#8-非转义序列的模板字符串)

## 1 异步迭代
```javascript
在async/await的某些时刻，你可能尝试在同步循环中调用异步函数。例如：

async function process(array) {
  for (let i of array) {
    await doSomething(i)
  }
}
这段代码不会正常运行，下面这段同样也不会：

async function process(array) {
  array.forEach(async i => {
    await doSomething(i)
  })
}
这段代码中，循环本身依旧保持同步，并在在内部异步函数之前全部调用完成。

ES2018引入异步迭代器（asynchronous iterators），这就像常规迭代器，除了next()方法返回一个Promise。
因此await可以和for...of循环一起使用，以串行的方式运行异步操作。例如：

async function process(array) {
  for await (let i of array) {
    doSomething(i)
  }
}
```
## 2 Promise.finally()
```javascript
一个Promise调用链要么成功到达最后一个.then()，要么失败触发.catch()。在某些情况下，你想要在无论Promise运行成功还是失败，
运行相同的代码，例如清除，删除对话，关闭数据库连接等。

.finally()允许你指定最终的逻辑：

function doSomething() {
  doSomething1()
  .then(doSomething2)
  .then(doSomething3)
  .catch(err => {
    console.log(err)
  })
  .finally(() => {
  // finish here!
  })
}
```

## 3 Rest/Spread属性
```javascript
ES2015引入了Rest参数和扩展运算符。三个点（...）仅用于数组。Rest参数语法允许我们将一个布丁数量的参数表示为一个数组。

restParam(1, 2, 3, 4, 5)
function restParam(p1, p2, ...p3) {
  // p1 = 1
  // p2 = 2
  // p3 = [3, 4, 5]
}
展开操作符以相反的方式工作，将数组转换成可传递给函数的单独参数。例如Math.max()返回给定数字中的最大值：

const values = [99, 100, -1, 48, 16]
console.log( Math.max(...values) ) // 100
ES2018为对象解构提供了和数组一样的Rest参数（）和展开操作符，一个简单的例子：

const myObject = {
  a: 1,
  b: 2,
  c: 3
}

const { a, ...x } = myObject
// a = 1
// x = { b: 2, c: 3 }
或者你可以使用它给函数传递参数：

restParam({
  a: 1,
  b: 2,
  c: 3
})

function restParam({ a, ...x }) {
  // a = 1
  // x = { b: 2, c: 3 }
}
跟数组一样，Rest参数只能在声明的结尾处使用。此外，它只适用于每个对象的顶层，如果对象中嵌套对象则无法适用。

扩展运算符可以在其他对象内使用，例如：

const obj1 = { a: 1, b: 2, c: 3 }
const obj2 = { ...obj1, z: 26 }
// obj2 is { a: 1, b: 2, c: 3, z: 26 }
可以使用扩展运算符拷贝一个对象，像是这样obj2 = {...obj1}，但是 这只是一个对象的浅拷贝。另外，
如果一个对象A的属性是对象B，那么在克隆后的对象cloneB中，该属性指向对象B。
```

## 4 正则表达式命名捕获组
```javascript
Regular Expression Named Capture Groups
JavaScript正则表达式可以返回一个匹配的对象——一个包含匹配字符串的类数组，例如：以YYYY-MM-DD的格式解析日期：

const reDate = /([0-9]{4})-([0-9]{2})-([0-9]{2})/,
  match = reDate.exec('2018-04-30'),
  year = match[1], // 2018
  month = match[2], // 04
  day = match[3] // 30
这样的代码很难读懂，并且改变正则表达式的结构有可能改变匹配对象的索引。

ES2018允许命名捕获组使用符号 ? /<name>，在打开捕获括号'('后立即命名，示例如下：

const reDate = /(?<year>[0-9]{4})-(?<month>[0-9]{2})-(?<day>[0-9]{2})/,
  match  = reDate.exec('2018-04-30'),
  year   = match.groups.year,  // 2018
  month  = match.groups.month, // 04
  day    = match.groups.day   // 30
任何匹配失败的命名组都将返回undefined。

命名捕获也可以使用在replace()方法中。例如将日期转换为美国的MM-DD-YYYY格式：

const reDate = /(?<year>[0-9]{4})-(?<month>[0-9]{2})-(?<day>[0-9]{2})/,
  d      = '2018-04-30',
  usDate = d.replace(reDate, '$<month>-$<day>-$<year>')
```

## 5 正则表达式反向断言(lookbehind)
```javascript
目前JavaScript在正则表达式中支持先行断言（lookahead）。这意味着匹配会发生，但不会有任何捕获，
并且断言没有包含在整个匹配字段中。例如从价格中捕获货币符号：

const reLookahead = /\D(?=\d+)/,
  match = reLookahead.exec('$123.89')

  console.log( match[0] ) // $
ES2018引入以相同方式工作但是匹配前面的反向断言（lookbehind），这样我就可以忽略货币符号，单纯的捕获价格的数字：

const reLookbehind = /(?<=\D)\d+/,
  match = reLookbehind.exec('$123.89')

  console.log( match[0] ) // 123.89
以上是肯定反向断言，非数字\D必须存在。同样的，还存在否定反向断言，表示一个值必须不存在，例如：

const reLookbehindNeg = /(?<!\D)\d+/,
  match = reLookbehind.exec('$123.89')

  console.log( match[0] ) // null
```

## 6 正则表达式dotAll模式
```javascript
正则表达式中点.匹配除回车外的任何单字符，标记s改变这种行为，允许行终止符的出现，例如：
/hello.world/.test('hello\nworld')  // false
/hello.world/s.test('hello\nworld') // true
```

## 7 正则表达式Unicode转义
```javascript
到目前为止，在正则表达式中本地访问Unicode字符属性是不被允许的。ES2018添加了Unicode属性转义——形式为\p{...}和\P{...}，
在正则表达式中使用标记u(unicode)设置，在\p块儿内，可以以键值对的方式设置需要匹配的属性而非具体内容。例如：

const reGreekSymbol = /\p{Script=Greek}/u
reGreekSymbol.test('π') // true

此特性可以避免使用特定Unicode 区间来进行内容类型判断，提升可读性和可维护性。

```

## 8 非转义序列的模板字符串
```javascript
之前，\u开始一个unicode转义，\x开始一个十六进制转义，\后跟一个数字开始一个八进制转义。
这使得创建特定的字符串变得不可能，例如Windows文件路径 C:\uuu\xxx\111。更多细节参考
[模板字符串](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/template_strings)
```