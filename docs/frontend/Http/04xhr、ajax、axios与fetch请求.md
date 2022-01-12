## 介绍

### xhr(即XMLHttpRequest)

> 是 `AJAX` 的基础，用于在后台与服务器交换数据

#### 1. 优点：

- 在不重新加载页面的情况下更新网页
- 在页面已加载后从服务器请求数据
- 在页面已加载后从服务器接收数据
- 在后台向服务器发送数据

#### 2. 缺点：

- 书写繁琐，使用不方便
- 如果有多个请求，并且有依赖关系的话，容易形成回调地狱。
- 对于ie6、ie5需要写兼容代码

```js
var xhr;
if (window.XMLHttpRequest) { // IE7+ 
  xhr = new XMLHttpRequest()
} else if (window.ActiveXObject) { // IE6、IE5
  xhr = new ActiveXObject('Microsoft.XMLHTTP')
}
xhr.open('POST', url, true)
xhr.send(data)
xhr.onreadystatechange = function () {
  try {
    if (xhr.readyState === 4) {
      if (xhr.status === 200) {
        // successed
        callback(xhr.responseText);
      } else {
        // failed
        console.error("status error: ", xhr.status);
      }
    } else {
      // Not ready yet
    }
  } catch (e) {
    alert('something wrong: ' + e);
  }
}
```

#### 3. 跨域支持度

不支持

### jQuery ajax

> 基于 `xhr` 的再封装，可以简单的使用 `$.get()` ，`$.post()` ，也可以完整书写

```js
$.ajax({
  type: 'POST',
  url: url,
  data: data,
  dataType: dataType,
  success: function () {},
  error: function () {}
})
```

#### 1. 优点：

- 对 `xhr` 的封装，做了兼容处理，简化了使用。
- `dataType` 增加 `JSONP` 的支持，可处理部分跨域。

#### 2. 缺点：

- 如果有多个请求，并且有依赖关系的话，容易形成回调地狱。
- 使用时需要引入 `jQuery` 。

#### 3. 跨域支持度

支持 `JSONP` 跨域

> tips: `JSONP` 跨域仅支持 `get` 请求，且存在安全隐患。

### axios

是一个基于 `promise` 的 `HTTP` 库，可用于浏览器与 `node.js` 中，本质还是对 `xhr` 的封装，但是 `promise` 版

```js

try {
  let res = await axios({
    method: "post",
    url: "xxx",
    data: data
  });
  console.log("response", res);
} catch(error) {
  console.log("something error: ", error);
}
```

#### 1. 优点：

- 支持 `Promise API`，解决了 “回调地狱” 问题。
- 提供了并发请求的接口(可同时发起多个请求)。
- 可拦截请求与响应。
- 自动转换 `JSON` 数据。
- 客户端支持防止 `CSRF`

> tips: 防止 `CSRF` , 就是让你的每个请求都带一个从 `cookie` 中拿到的 `key` , 后台就可以据此轻松辨别出这个请求是否是用户在假冒网站上的误导输入。

#### 2. 缺点：

- 只支持现代浏览器

#### 3. 跨域支持度

### fetch

使用原生 `js` 实现，不再使用 `xhr` 对象。

```js
try {
  let response = await fetch(url);
  // successed
  let data = response.json();
} catch(error) {
  console.log("something error: ", error);
}
```

#### 1. 优点：

- 支持 `Promise API` 。
- 脱离了 `xhr` ，是一种新的实现方式。

#### 2. 缺点：

- `fetch` 只对网络请求报错，对于状态码为400、500都当做成功的请求，需要再次封装
- `fetch` 默认不会带 `cookie` ，需要添加配置项： `fetch(url, {credentials: "include"})` 。
- 不支持abort，不支持超时控制
- 无法监测请求的进度

#### 3. 跨域支持度

在配置中，添加 `mode: "no-cors"` 就可以跨域了

```js
fetch(url, {
    method: "post", 
    mode: "no-cors",
    data: {}
}).then(() => { /* handle response */ });
```

## 区别

- `jQuery ajax` 与 `axios` ：
  - 相同点：
    - 在 `xhr` 基础上的改进版
    - 简化书写方式
  - 不同点：
    - 前者依赖于 `jQuery` ，后者在 `xhr` 基础上，结合使用 `Promise` 。
    - 后者可拦截请求与响应，且解决了 “回调地狱” 问题。
- `xhr` 与 `fetch` 的：
  - 相同点：
    - 需要对状态码判断处理，以确定请求响应状态
  - 不同点：
    - 实现方式不同：前者基于 `ajax` ，后者由原生 `js` 实现。