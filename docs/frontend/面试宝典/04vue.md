## 父子组件生命周期的关系

子组件的生命周期在父组件 `beforeMounte` 之后触发

~~~javascript
parent beforeCreate;
parent created;
parent beforeMount;
child beforeCreate;
child created;
child beforeMount;
child mounted;
parent mounted;
~~~

## vue data为什么是函数而不是对象

为对象的话，同一个组件被不同页面使用时，共享的是同一个 `state` ，所以用的是函数。

