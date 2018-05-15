name:inverse
class: center, middle,inverse
layout: true

---

# 跨域

Fighting
???

当我们在聊跨域的时候，我们在聊什么？

* 报错
* JSONP
* CORS(Cross-Origin Resource Sharing)

旨在由浅入深，让之前不懂跨域的小伙伴知道跨域是怎么回事，让已经知道跨域的小伙伴，知道如何跨域，与深谙跨域之道的小伙伴探讨一下一些我的思考。

---

* ###同源策略

--

* ###限制原因

--

* ###HTTP 协议

--

* ###如何跨域

???

为什么要从这些方面聊跨域问题

学习任何技术都应该以如此心态及规划

在知识广度够的情况下，加深深度

---

# 同源策略

---

.left-column[

## 同源

]
.right-column[
.left[

> 如果两个页面的协议、端口、域名都相同，则两个页面具有相同的源。

]
]

---

.left-column[

## 同源

## 策略

]
.right-column[
.left[

> 同源策略限制了从同一个源加载的文档或脚本如何与来自另一个源的资源进行交互。这是一个用于隔离潜在恶意文件的重要安全机制。

* 数据访问
* 网络访问

]]

???

表现在：

本地数据访问控制：

* cookie
* storage
* indexDB
* web 字体
* canvas 加载图片
* webGL
* DOM

网络访问控制（JSONP 算是一种取巧）
img、script、link、video、object、iframe

---

.left-column[

## 同源

## 策略

## 🌰

]
.right-column[
.left.right-img[

URL:http://www.example.com/dir/page.html

![同源策略](./images/same-origin-policy.png)

]
]

---

# 限制原因

???

集思广益参与回答

---

.left-column[

## 假如

]
.right-column[
.left.right-img[

![假如允许跨域](./images/cross-dmain.png)

]
]

---

.left-column[

## 假如

## 疑惑

]
.right-column[
.left[

* 以上的攻击基于 cookie 的跨域共享，既然是 cookie 的不安全导致了跨域的不安全，仅仅禁止跨域共享 cookie 就好了，为何要限制跨域网络的访问？
* 跨域限制存在的情况下，跨域请求是否真的无法发送？

]
]

???

* ###ip 鉴权场景
* ###免费的分布式爬虫

### 跨域请求是否真的无法发送？ 是，也不是

* 资源加载 img,css,script,video...
* form 表单提交
* 普通 ajax 请求

抓包演示

https 向 http 的请求会被浏览器拦截

---

# HTTP 协议

???
RFC、HTTP 权威指南

* 形态
* 连接
* 网关
* 认证
* 代理
* 缓存
* 安全
* 编码

不做细致讲解

---

.left-column[

## 协议

]
.right-column[
.left.right-img[
![白云黑土](./images/baiyunheitu.jpg)
]
]

???

喝水、闭嘴、揉腿、亲一口老嘴

---

.left-column[

## 协议

## 请求

]
.right-column[
.left[

```
GET /resources/public-data/ HTTP/1.1
Host: bar.other
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.170 Safari/537.36
Accept: text/html,application/xhtml+xml,application/json;q=0.9,*/*;q=0.8
Accept-Language: en-us,en;q=0.5
Accept-Encoding: gzip,deflate
Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.7
Connection: keep-alive
Referer: http://foo.example/examples/access-control/simpleXSInvocation.html
Origin: http://foo.example
```

]
]

---

.left-column[

## 协议

## 请求

## 响应

]
.right-column[
.left[

```
HTTP/1.1 200 OK
Date: Mon, 01 Dec 2008 00:23:53 GMT
Server: Apache/2.0.61
Access-Control-Allow-Origin: *
Keep-Alive: timeout=2, max=100
Connection: Keep-Alive
Transfer-Encoding: chunked
Content-Type: application/json

{"msg":"success",code:10}
```

]
]

---

# 如何跨域

--

### CORS(Cross-Origin Resource Sharing)

???

被滥用的 jsonp
被滥用的 cookie

* JSONP
* document.domain(主域相同)
* document.name
* navigator 跨域 （IE6、7）
* message 机制 (HTML5)
* ...

跨域只是针对浏览器的策略，因为浏览器是公共环境。

---

.left-column[

## 请求头

]
.right-column[
.left[

* Access-Control-Request-Headers
* Access-Control-Request-Method

  ]]

???

这两个请求头只出现在预检请求中，也就是我们经常碰到的 options 请求

浏览器自发行为

---

.left-column[

## 请求头

## 响应头

]
.right-column[
.left[

* Access-Control-Allow-Headers
* Access-Control-Request-Method
* Access-Control-Allow-Origin
* Access-Control-Expose-Headers
* Access-Control-Max-Age
* Access-Control-Allow-Credentials

]]

???

* 除了 Access-Control-Allow-Origin，其它的响应头均未实现\*号
* Access-Control-Allow-Headers 中以下是默认支持的，不需要列出来：Accept、Accept-Language、Content-Language、Content-Type。但 Content-Type 只能是 application/x-www-form-urlencoded、multipart/form-data 或 text/plain
* Access-Control-Allow-Origin 为\*的情况下，不能设置 Access-Control-Allow-Credentials（：为安全操碎了心的委员会

---

# 预检请求

--

### preflight

---

.left-column[

## 描述

]
.right-column[
.left[

> 跨域资源共享标准新增了一组 HTTP 首部字段，允许服务器声明哪些源站有权限访问哪些资源。另外，规范要求，对那些可能对服务器数据产生副作用的 HTTP 请求方法（特别是 GET 以外的 HTTP 请求，或者搭配某些 MIME 类型的 POST 请求）（非简单请求），浏览器必须首先使用 OPTIONS 方法发起一个预检请求（preflight request），从而获知服务端是否允许该跨域请求。服务器确认允许之后，才发起实际的 HTTP 请求。在预检请求的返回中，服务器端也可以通知客户端，是否需要携带身份凭证（包括 Cookies 和 HTTP 认证相关数据）。

]]

---

.left-column[

## 描述

## Simple Requests

]
.right-column[
.left[
满足如下条件：

* 请求方法：GET、HEAD、POST
* 自定义头：Accept、Accept-Language、Content-Language、Content-Type
* Content-Type 仅限值：text/plain、multipart/form-data、application/x-www-form-urlencoded
* 请求中的任意 XMLHttpRequestUpload 对象均没有注册任何事件监听器
* 请求中没有使用 ReadableStream 对象。

]]

???

* ReadableStream 只有 chrome 实现了
* XMLHttpRequestUpload 对象可以使用 XMLHttpRequest.upload 属性访问。

---

.left-column[

## 描述

## Simple Requests

## Preflight

]
.right-column[
.left[

    ![prelight](./images/prelight.png)

]]

???

预检请求被重定向会出错

---

```
    const review = [
        '何为同源策略',
        '为何限制跨域',
        'HTTP协议',
        'CORS'
    ]
```

---

```
    console.log("Thanks!");
```
