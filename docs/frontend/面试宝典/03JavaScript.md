## Object.create() 创建的对象和普通对象的区别

> [https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create)
>
> **`Object.create()`**方法创建一个新对象，使用现有的对象来提供新创建的对象的 `__proto__`。

使用 `let obj = {}` 方式构建的对象默认 `__proto__` 指向 `Object`。使用 `Object.create` 方法允许我们指定实例对象的 `__proto__` 指向对象。 

## 浅拷贝与深拷贝

- 浅拷贝是拷贝的值
- 深拷贝拷贝的是值与址

### 如何实现深拷贝

~~~javascript
const deepClone = function (obj) {
  let newObj;
  if (Array.isArray(obj)) {
    newObj = obj.map(item => deepClone(item));
  } else if (Object.prototype.toString.call(obj) === "[object Object]") {
    newObj = {};
    for (key in obj) {
      newObj[key] = deepClone(obj[key]);
    }
  } else {
    newObj = obj;
  }
  return newObj;
}
~~~

## js事件机制

谷歌浏览器在触发事件时触发 `捕获` 机制：由顶层到里层节点（document -> body -> div） 

ie 浏览器在触发事件时触发 `冒泡` 机制：由里层到顶层节点（div->body->document）

浏览器的事件使用的是 `发布-订阅者` 模式：通过 `window.addEventListener` 订阅对应的事件，在触发对应事件时，由发布者更新发布事件。