## Object.create() 创建的对象和普通对象的区别

> [https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create)
>
> **`Object.create()`**方法创建一个新对象，使用现有的对象提供新给创建的新对象的 `__proto__`。

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

谷歌浏览器在触发事件时触发 `捕获` 机制：由顶层到里层节点（document -> body -> div） 

ie 浏览器在触发事件时触发 `冒泡` 机制：由里层到顶层节点（div->body->document）

浏览器的事件使用的是 `发布-订阅者` 模式：通过 `window.addEventListener` 订阅对应的事件，在触发对应事件时，由发布者更新发布事件。

## js Event loop

参考阮一峰的文档：[http://www.ruanyifeng.com/blog/2014/10/event-loop.html](http://www.ruanyifeng.com/blog/2014/10/event-loop.html) 

~~~
（1）所有同步任务都在主线程上执行，形成一个执行栈（execution context stack）。

（2）主线程之外，还存在一个"任务队列"（task queue）。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件。

（3）一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。

（4）主线程不断重复上面的第三步。
~~~

### 微任务与宏任务

目前 js 执行环境有两种： `node`、`浏览器` 。由执行环境规定的异步执行任务类似 `setTimeout` 、`setInterval` 被称为宏任务；而由语言标准 `ECMScript` 提供的类似 `Promise` 、`Generator` 别称为微任务；

要点：

- 微任务会比宏任务优先执行