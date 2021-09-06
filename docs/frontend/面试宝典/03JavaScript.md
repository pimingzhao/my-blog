## Object.create() 创建的对象和普通对象的区别

> [https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create)
>
> `Object.create()` 方法创建一个新对象，使用现有的对象提供新给创建的新对象的 `__proto__`。

使用 `let obj = {}` 方式构建的对象默认 `__proto__` 指向 `Object`。使用 `Object.create` 方法允许我们指定实例对象的 `__proto__` 指向对象。 

## 浅拷贝与深拷贝

- 浅拷贝是拷贝的值
- 深拷贝拷贝的是值与址

### 如何实现深拷贝

1. 通过 json API

~~~javascript
const deepClone = function (obj) { return JSON.parse(JSON.stringify(obj))) };
// 要点：JSON.stringify() 转字符串是对于值为 undefined 的对象不会保留，而是会删除这个键值对
~~~

2. 通过手写 function

~~~javascript
const deepClone = function (obj) {
  if (Array.isArray(obj)) {
    return obj.map(item => deepClone(item));
  }
  if (Object.prototype.toString.call(obj) === "[object Object]") {
    const newObj = {};
    for (key in obj) {
      newObj[key] = deepClone(obj[key]);
    }
    return newObj;
  }
  return  obj;
}
~~~

## js事件机制

- 事件捕获：事件触发由外至内
- 事件冒泡：事件触发由内至外

同一个元素同时绑定捕获与冒泡时，w3c 标准规定执行顺序为：事件捕获 -> 事件冒泡

如何规定事件触发机制：

~~~javascript
element.addEventListener(event, function, useCapture)
// useCapture 默认值为false：事件句柄在冒泡阶段执行；值为 true 事件句柄在捕获阶段执行
~~~

## js Event loop

参考阮一峰的文档：[http://www.ruanyifeng.com/blog/2014/10/event-loop.html](http://www.ruanyifeng.com/blog/2014/10/event-loop.html) 

~~~
（1）所有同步任务都在主线程上执行，形成一个执行栈（execution context stack）。

（2）主线程之外，还存在一个"任务队列"（task queue）。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件。

（3）一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。

（4）主线程不断重复上面的第三步。
~~~

### 微任务与宏任务

目前 js 执行环境有两种： `node`、`浏览器` 。由执行环境规定的异步执行任务类似 `setTimeout` 、`setInterval` 被称为宏任务；而由语言标准 `ECMScript` 提供的类似 `Promise` 、`Generator` 异步事件执行为微任务；

要点：

- 执行时间相同时微任务会比宏任务优先完成：当执行栈执行完同步代码后，优先检查是否有微任务队列，其次检查是否有宏任务队列。

~~~javascript
// demo 来源：https://www.zhihu.com/search?type=content&q=js%E5%BE%AE%E4%BB%BB%E5%8A%A1%E4%B8%8E%E5%AE%8F%E4%BB%BB%E5%8A%A1
console.log(1)

setTimeout(function() {
  console.log(2)
})

new Promise(function (resolve) {
  console.log(3)
  resolve()
}).then(function () {
  console.log(4)
}).then(function() {
  console.log(5)
})

console.log(6)
// 执行结果：1 3 6 4 5 2
~~~

## 普通函数与箭头函数的区别

- this 指向不一样
  - 普通函数 this 指向函数本身；箭头函数自身无 this，所以 this 会由原型链向上查找
- 箭头函数没有 arguments 对象
  - 普通函数可以通过 arguments 对象获取函数参数

## js继承

参考另一篇文章：[原型与原型链、继承](../JavaScript/01原型与原型链、继承#继承.md)

## 函数柯里化

~~~javascript
// 函数柯里化
function add (a, b, c, d) {
  return a + b + c + d
}

function currey(fn) {
  var length = fn.length
  var args = Array.prototype.slice.call(arguments, 1)

  return function () {
    var nweArgs = args.concat(Array.prototype.slice.call(arguments))
    if (nweArgs.length < length) {
      return currey.apply(this, [fn].concat(nweArgs))
    }
    return fn.apply(this, nweArgs)
  }
}

currey(add, 1, 2, 3, 4) // 10
currey(add, 1)()(2)(3, 4) // 10
~~~

