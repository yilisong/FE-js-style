# es8中常用知识

* [1 async/await](#1-async/await)
* [2 Object.values()](#2-Object.values())
* [3 Object.entries()](#3-Object.entries())
* [4 padStart()和padEnd()](#4-padStart()和padEnd())
* [5 函数参数列表结尾允许逗号](#5-函数参数列表结尾允许逗号)
* [6 Object.getOwnPropertyDescriptors()](#6-Object.getOwnPropertyDescriptors())
* [7 ShareArrayBuffer对象](#7-ShareArrayBuffer对象)
* [8 Atomics对象](#8-Atomics对象)


## 1 async/await
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

```

## 2 Object.values()
```javascript
Object.values()是一个与Object.keys()类似的新函数，但返回的是Object自身属性的所有值，不包括继承的值。

假设我们要遍历如下对象obj的所有值：

const obj = {a: 1, b: 2, c: 3}
不使用Object.values() :ES7

const vals=Object.keys(obj).map(key=>obj[key])
console.log(vals)//[1, 2, 3]
使用Object.values() :ES8

const values=Object.values(obj1)
console.log(values)//[1, 2, 3]
从上述代码中可以看出Object.values()为我们省去了遍历key，并根据这些key获取value的步骤。
```

## 3 Object.entries()
```javascript
Object.entries()函数返回一个给定对象自身可枚举属性的键值对的数组。

接下来我们来遍历上文中的obj对象的所有属性的key和value：

不使用Object.entries() :ES7

Object.keys(obj).forEach(key=>{
  console.log('key:'+key+' value:'+obj[key])
})
//key:a value:1
//key:b value:2
//key:c value:3
使用Object.entries() :ES8

for(let [key,value] of Object.entries(obj1)){
  console.log(`key: ${key} value:${value}`)
}
//key:a value:1
//key:b value:2
//key:c value:3

```
## 4 padStart()和padEnd()
```javascript
在ES8中String新增了两个实例函数String.prototype.padStart和String.prototype.padEnd，
允许将空字符串或其他字符串添加到原始字符串的开头或结尾。

String.padStart(targetLength,[padString])

targetLength:当前字符串需要填充到的目标长度。如果这个数值小于当前字符串的长度，则返回当前字符串本身。
padString:(可选)填充字符串。如果字符串太长，使填充后的字符串长度超过了目标长度，则只保留最左侧的部分，
其他部分会被截断，此参数的缺省值为 ' '。
console.log('0.0'.padStart(4,'10')) //10.0
console.log('0.0'.padStart(20))// 0.00    
String.padEnd(targetLength,padString])
console.log('0.0'.padEnd(4,'0')) //0.00    
console.log('0.0'.padEnd(10,'0'))//0.00000000
  
```

## 5 函数参数列表结尾允许逗号
```javascript
主要作用是方便使用git进行多人协作开发时修改同一个函数减少不必要的行变更。如：
function process(array) {
  for (let i of array) {
    doSomething(i)
  }
},

```

## 6 Object.getOwnPropertyDescriptors()
```javascript
Object.getOwnPropertyDescriptors()函数用来获取一个对象的所有自身属性的描述符,如果没有任何自身属性，则返回空对象。

函数原型：
Object.getOwnPropertyDescriptors(obj)
返回obj对象的所有自身属性的描述符，如果没有任何自身属性，则返回空对象。

const obj2 = {
  name: 'Jine',
  get age() { return '18' }
}
Object.getOwnPropertyDescriptors(obj2)
// {
//   age: {
//     configurable: true,
//     enumerable: true,
//     get: function age(){}, //the getter function
//     set: undefined
//   },
//   name: {
//     configurable: true,
//     enumerable: true,
//		value:'Jine',
//		writable:true
//   }
// }
```

## 7 ShareArrayBuffer对象
```javascript
SharedArrayBuffer 对象用来表示一个通用的，固定长度的原始二进制数据缓冲区，类似于 ArrayBuffer 对象，
它们都可以用来在共享内存（shared memory）上创建视图。与 ArrayBuffer 不同的是，SharedArrayBuffer 不能被分离。
/**
* 
* @param {*} length 所创建的数组缓冲区的大小，以字节(byte)为单位。  
* @returns {SharedArrayBuffer} 一个大小指定的新 SharedArrayBuffer 对象。其内容被初始化为 0。
*/
new SharedArrayBuffer(length)
```

## 8 Atomics对象
```javascript
Atomics 对象提供了一组静态方法用来对 SharedArrayBuffer 对象进行原子操作。

这些原子操作属于 Atomics 模块。与一般的全局对象不同，Atomics 不是构造函数，因此不能使用 new 操作符调用，
也不能将其当作函数直接调用。Atomics 的所有属性和方法都是静态的（与 Math 对象一样）。

多个共享内存的线程能够同时读写同一位置上的数据。原子操作会确保正在读或写的数据的值是符合预期的，
即下一个原子操作一定会在上一个原子操作结束后才会开始，其操作过程不会中断。

Atomics.add()
将指定位置上的数组元素与给定的值相加，并返回相加前该元素的值。

Atomics.and()
将指定位置上的数组元素与给定的值相与，并返回与操作前该元素的值。

Atomics.compareExchange()
如果数组中指定的元素与给定的值相等，则将其更新为新的值，并返回该元素原先的值。

Atomics.exchange()
将数组中指定的元素更新为给定的值，并返回该元素更新前的值。

Atomics.load()
返回数组中指定元素的值。

Atomics.or()
将指定位置上的数组元素与给定的值相或，并返回或操作前该元素的值。

Atomics.store()
将数组中指定的元素设置为给定的值，并返回该值。

Atomics.sub()
将指定位置上的数组元素与给定的值相减，并返回相减前该元素的值。

Atomics.xor()
将指定位置上的数组元素与给定的值相异或，并返回异或操作前该元素的值。

wait() 和 wake() 方法采用的是 Linux 上的 futexes 模型（fast user-space mutex，快速用户空间互斥量），
可以让进程一直等待直到某个特定的条件为真，主要用于实现阻塞。

Atomics.wait()
检测数组中某个指定位置上的值是否仍然是给定值，是则保持挂起直到被唤醒或超时。返回值为 'ok'、'not-equal' 或 'time-out'。
调用时，如果当前线程不允许阻塞，则会抛出异常（大多数浏览器都不允许在主线程中调用 wait()）。

Atomics.wake()
唤醒等待队列中正在数组指定位置的元素上等待的线程。返回值为成功唤醒的线程数量。

Atomics.isLockFree(size)
可以用来检测当前系统是否支持硬件级的原子操作。对于指定大小的数组，如果当前系统支持硬件级的原子操作，
则返回 true；否则就意味着对于该数组，Atomics 对象中的各原子操作都只能用锁来实现。此函数面向的是技术专家。
```
