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

