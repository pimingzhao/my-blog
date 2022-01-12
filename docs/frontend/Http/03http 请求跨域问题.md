## 1. 跨域产生原因

浏览器同源策略限制（**DOM同源策略**、**XmlHttpRequest同源策略**）

## 2. http 跨域解决方案

### 2.1 JSONP

js 脚本可以不受跨域限制，所以通过使用 script 标签来进行跨域请求，例如：

```
// 8080 端口向 9090 请求数据
const script = "<script type='text/javascript' src='http://localhost:9090/userList?callBack=getData'><\/script>";
$("head").append(script);
```

使用 jquery 指定返回类型为 jsonp 即可。更多关于 jsonp 实现的知识可查看 [jsonp 为什么不支持 post](https://www.jianshu.com/p/08e7732e3333)

> 但 JSONP 处理跨域有限制：只支持 get 请求方式。通过 script 标签只指定了请求的 url，无法向服务端传输数据

### 2.2 CORS

> CORS跨资源共享，由W3C推荐的方案，能使服务器支持 XmlHttpRequest 的跨域请求；使用CORS，只需要添加一些 HTTP 头，让服务器申明允许访问的来源

- 使用 CORS 时，异步请求会被分为简单请求与非简单请求。[点击查看更多关于简单、非简单请求知识](https://blog.csdn.net/yexudengzhidao/article/details/100104134)
- 同时满足以下条件的我们称之为简单请求，除此之外均为非简单请求

1. 请求方式为 get、post、head
2. HTTP的头信息不超出以下几种字段：
   - Accept
   - Accept-Language
   - Content-Language
   - Last-Event-ID
   - Content-Type：只限于三个值 application/x-www-form-urlencoded、multipart/form-data、text/plain

> 在发起非简单请求时，浏览器会先发起预检请求（OPTIONS）：浏览器询问服务端当前网页所在域是否被允许跨域，以及被允许使用的请求方式和头信息字段，预检通过，表示允许跨域。

## 3. 请求跨域案例：

### 3.1 由 withCredentials 属性引起的跨域

当在前端请求中设置了 `withCredentials` 属性为 `true` 时，后台 `response header` 属性 `Access-Control-Allow-Origin` 必须指定当前跨域的 **`域`** （当前为 * 即允许所有域请求），否则会报以上的请求错误

```
Access to XMLHttpRequest at 'http://域名:8081/userList' from origin 'http://域名:8080' has been blocked by CORS policy: The value of the 'Access-Control-Allow-Origin' header in the response must not be the wildcard '*' when the request's credentials mode is 'include'. The credentials mode of requests initiated by the XMLHttpRequest is controlled by the withCredentials attribute.
```

解决方案：

```
// 指定当前跨域的域
Access-Control-Allow-Origin", "http://域名:8080"
```

## 4. XMLHTTPRequest 属性

详情参考 [XMLHTTPRequest属性、方法、事件大全&详解](https://segmentfault.com/a/1190000019891237)
常见的有：status、responseType
此外还有

- readyState：只读，用于追踪`xhr`当前的状态值，从 0 到 4 分别标识服务端初始化状态；open方法已调用；sent方法已调用；客户端响应头接收完成，响应体开始接收；HTTP响应已完全接收
- responseText：当 `responseType`为`text`、`""` 时，才可使用
- statusText：表示`HTTP`响应状态的描述文本，如`OK`、`Not Found`等
- upload： 一个 `XMLHttpRequestUpload` 对象，用于收集传输信息。支持除 `onreadystatechange` 外 xhr 的事件回调
- timeout: 超时时间设置，默认 0ms ，即不设置
- withCredentials ：Boolean 类型，指示是否携带类似 cookies, authorization 或者TLS客户端证书跨域访问控制，默认值为false 即不携带。在同一域下设置该属性值无效