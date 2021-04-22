## Object.create() 创建的对象和普通对象的区别

> [https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create)
>
> **`Object.create()`**方法创建一个新对象，使用现有的对象来提供新创建的对象的 `__proto__`。

使用 `let obj = {}` 方式构建的对象默认 `__proto__` 指向 `Object`。使用 `Object.create` 方法允许我们指定实例对象的 `__proto__` 指向对象。 