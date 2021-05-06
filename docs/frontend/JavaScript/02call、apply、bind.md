## 什么是this

借用 MDN 对 this 的说明：[https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/this](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/this)

在不同内容中，this 代指的内容是不一样的，而通过 call/apply/bind 方法，我们可以更改 this 的指向，以达到我们的预期

## call 与 apply

- call(thisArg, arg1, arg2)
- apply(thisArg, arguments)

## bind

实现一个 bind ：

~~~javascript
var myBind = function (context) {
  var that = this;
  var args = Array.prototype.slice.call(arguments, 1);
  var fBound = function () {
    const bindArgs = Array.prototype.slice.call(arguments);
    // fBound 为构造函数时，this 指向函数本身
    return that.apply(this instanceof fBound ? this : context, args.concat(bindArgs));
  }
  fBound.prototype = this.prototype;
  return fBound;
}
~~~

