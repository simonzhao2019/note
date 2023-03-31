### 1、http协议概述

HTTP是一个客户端（用户）和服务端（网站）之间请求和应答的标准，通常使用TCP协议。通过使用网页浏览器、网络爬虫或者其它的工具，客户端发起一个HTTP请求到服务器上指定端口（默认端口为80）。我们称这个客户端为用户代理程序（user agent）。应答的服务器上存储着一些资源，比如HTML文件和图像。我们称这个应答服务器为源服务器（origin server）。在用户代理和源服务器中间可能存在多个“中间层”，比如代理服务器、网关或者隧道（tunnel）。

尽管TCP/IP协议是互联网上最流行的应用，但是在HTTP协议中并没有规定它必须使用或它支持的层。事实上HTTP可以在任何互联网协议或其他网络上实现。HTTP假定其下层协议提供可靠的传输。因此，任何能够提供这种保证的协议都可以被其使用，所以其在TCP/IP协议族使用TCP作为其传输层。

通常，由HTTP客户端发起一个请求，创建一个到服务器指定端口（默认是80端口）的TCP连接。HTTP服务器则在那个端口监听客户端的请求。一旦收到请求，服务器会向客户端返回一个状态，比如"HTTP/1.1 200 OK"，以及返回的内容，如请求的文件、错误消息、或者其它信息。

http目前有三个协议版本（IETF制定RFC  ：Request for Comments:  ）

1、http0.9，此时的版本没有形成规范，HTTP在应用的早期阶段非常简单，后来被称为HTTP/0.9，

2、http1.0   文档 RFC 1945 定义了 HTTP/1.0 （大量模糊定义，导致各种实现不统一）

3、htt1.1  1999年6月公布的 [RFC 2616](https://tools.ietf.org/html/rfc2616)，定义了HTTP协议中现今广泛使用的一个版本——HTTP 1.1。目前许多网站使用的仍然是这个协议  （ RFC 6265定义了cookie相关，包括set-cookie)

4.http2.0 当前版本，于2015年5月作为互联网标准正式发布



### 2、http协议中定义的请求方式

1、GET
向指定的资源发出“显示”请求。使用GET方法应该只用在读取资料，而不应当被用于产生“副作用”的操作中，例如在网络应用程序中。其中一个原因是GET可能会被网络爬虫等随意访问。参见安全方法。浏览器直接发出的GET只能由一个url触发。GET上要在url之外带一些参数就只能依靠url上附带querystring。

2、post

向指定资源提交数据，请求服务器进行处理（例如提交表单或者上传文件）。数据被包含在请求本文中。这个请求可能会创建新的资源或修改现有资源，或二者皆有。每次提交，表单的数据被浏览器用编码到HTTP请求的body里。浏览器发出的POST请求的body主要有两种格式，一种是application/x-www-form-urlencoded用来传输简单的数据，大概就是"key1=value1&key2=value2"这样的格式。另外一种是传文件，会采用multipart/form-data格式。采用后者是因为application/x-www-form-urlencoded的编码方式对于文件这种二进制的数据非常低效。

3、PUT

向指定资源位置上传其最新内容。

4、DELETE

请求服务器删除Request-URI所标识的资源。

5、TRACE

回显服务器收到的请求，主要用于测试或诊断。

6、OPTIONS

这个方法可使服务器传回该资源所支持的所有HTTP请求方法。用'*'来代替资源名称，向Web服务器发送OPTIONS请求，可以测试服务器功能是否正常运作。

7、CONNECT

HTTP/1.1协议中预留给能够将连接改为隧道方式的代理服务器。通常用于SSL加密服务器的链接（经由非加密的HTTP代理服务器）。

方法名称是区分大小写的。当某个请求所针对的资源不支持对应的请求方法的时候，服务器应当返回[状态码405](https://zh.wikipedia.org/wiki/HTTP状态码#405)（Method Not Allowed），当服务器不认识或者不支持对应的请求方法的时候，应当返回[状态码501](https://zh.wikipedia.org/wiki/HTTP状态码#501)（Not Implemented）。

### 3、CROS



跨源资源共享标准新增了一组 HTTP 首部字段，允许服务器声明哪些源站通过浏览器有权限访问哪些资源。另外，规范要求，对那些可能对服务器数据产生副作用的 HTTP 请求方法（特别是 GET 以外的 HTTP 请求，或者搭配某些 MIME 类型的 POST 请求），浏览器必须首先使用 OPTIONS 方法发起一个预检请求（preflight request），从而获知服务端是否允许该跨源请求。服务器确认允许之后，才发起实际的 HTTP 请求。在预检请求的返回中，服务器端也可以通知客户端，是否需要携带身份凭证（包括 Cookies 和 HTTP 认证相关数据）。

1、为什么会有CROS

To allow sharing responses cross-origin and allow for more versatile [fetches](https://fetch.spec.whatwg.org/#concept-fetch) than possible with HTML’s `form` element, the CORS protocol exists. It is layered on top of HTTP and allows responses to declare they can be shared with other [origins](https://html.spec.whatwg.org/multipage/origin.html#concept-origin).

为了允许跨域分享除了Html表单元素之外更多的元素。才有了 CORS 一系列协定的存在，它位于 HTTP 之上，并允许响应声明它们可以与其他来源共享。

CORS 协议由一组标头组成，这些标头指示响应是否可以跨域共享。

对于比 HTML 的表单元素更复杂的请求，执行 CORS 预检请求，以确保请求的当前 URL 支持 CORS 协议。

2、预检请求

“需预检的请求”要求必须首先使用 [`OPTIONS`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/OPTIONS)到服务器，以获知服务器是否允许该实际请求。"预检请求“的使用，可以避免跨域请求对服务器的用户数据产生未预期的影响。

大多数浏览器不支持针对于预检请求的重定向。如果一个预检请求发生了重定向，浏览器将报告错误：

3、关于跨域当中http请求的几个关键头部

**A)请求部分**

（1）withCredentials：是否携带身份凭证

[`XMLHttpRequest`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest).org/en-US/docs/Web/API/Fetch_API) 与 CORS 的一个有趣的特性是，可以基于  [HTTP cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies) 和 HTTP 认证信息发送身份凭证。一般而言，对于跨源 [`XMLHttpRequest`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest)la.org/en-US/docs/Web/API/Fetch_API) 请求，浏览器**不会**发送身份凭证信息。如果要发送凭证信息，需要设置 `XMLHttpRequest `的某个特殊标志位。

简单来说，我们只有将withCredentials` 标志设置为 `true，才能服务器发送 Cookies（第三方）。还有就是如果服务器端的响应中未携带 `Access-Control-Allow-Credentials: true` ，浏览器将不会把响应内容返回给请求的发送者。

（2）Access-Control-Request-Headers

请求头 **`Access-Control-Request-Headers `**出现于 [preflight request](https://developer.mozilla.org/zh-CN/docs/Glossary/Preflight_request) （预检请求）中，用于通知服务器在真正的请求中会采用哪些请求头。（不用自己定义，预检请求自己发送）

**B)响应部分**

（1） Access-Control-Allow-Origin 

 Access-Control-Allow-Origin 响应头指定了该响应的资源是否被允许与给定的[origin](https://developer.mozilla.org/zh-CN/docs/Glossary/Origin)共享，“`*`”表示所有域都具有访问资源的权限。这里我们需要注意的是如果withCredentials设置为true，则Access-Control-Allow-Origin不能设置为通配符*，必须指定域名。简单来说，对于不需要携带身份凭证的请求，我们可以把这个字段设置为通配符*。但是如果需要身份凭证，则必须指定域名

（2）Access-Control-Allow-Credentials

Access-Control-Allow-Credentials 头指定了当浏览器的credentials设置为true时是否允许浏览器读取response的内容。当用在对preflight预检测请求的响应中时，它指定了实际的请求是否可以使用credentials。请注意：简单 GET 请求不会被预检；如果对此类请求的响应中不包含该字段，这个响应将被忽略掉，并且浏览器也不会将相应内容返回给网页。

（3）Access-Control-Expose-Headers

在跨源访问时，XMLHttpRequest对象的getResponseHeader()方法只能拿到一些最基本的响应头，Cache-Control、Content-Language、Content-Type、Expires、Last-Modified、Pragma，如果要访问其他头，则需要服务器设置本响应头。具体在项目hls.js当中，针对视频的跨域，后端返回的x-cookies。后端，如果不在响应头中设置Access-Control-Expose-Headers：x-cookies，则前端在获取响应头的时候是拿不到x-cookies

（4）Access-Control-Allow-Headers

Access-Control-Allow-Headers 首部字段用于预检请求的响应。其指明了实际请求中允许携带的首部字段。

（5）Access-Control-Allow-Methods

Access-Control-Allow-Methods 首部字段用于预检请求的响应。其指明了实际请求所允许使用的 HTTP 方法。

3、第三方cookie问题

（2）服务的响应头中需要携带Access-Control-Allow-Credentials并且为true。

（2）响应头中的Access-Control-Allow-Origin一定不能为*，必须是指定的域名

（3）浏览器发起ajax需要指定withCredentials 为true

（4）注意sameSite问题（还需要注意chrome浏览器与fireFox浏览器的不同）

### **4、常见请求头**

#### （1）content-Type

content-Type首部字段用来说明了实体主体的MIME类型，MIME类型我们可以把它理解为作为货物运载的基本媒体类型。客户端用MIME类型来解释和处理其内容。MIME类型由一个主体媒体类型（如application、text等）后面跟一个斜杠以及一个子类型组成，子类型用来进一步描述媒体类型。比如text/html。

content-Type首部还支持可选的参数来进一步说明媒体的类型，如charset=UTF-8就用来表明把实体中的比特转换为文本文件中字符的方法

长让人疑惑的问题

**a、application类型**

经常看到的application/x-www-form-urlencoded

翻阅mdn，上面的解释。application以某种方式执行或解释的数据或需要特定应用程序或应用程序类别才能使用的二进制数据称为application。

```
The "application" top-level type is to be used for discrete data that
   do not fit under any of the other type names, and particularly for
   data to be processed by some type of application program. 
   
   							rfc6838对于application的官方定义
```

2、关于msip-admin里面请求头设置的疑惑

```js
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Cookie: connect.sid=s%3Ax4oGbzvQXZEM4HGEpCoUFyH4S7fci9XY.ria1%2BPmJwGU1UgxzA7vm6fhrlmn5hc%2F5SfkhhxSyLyk
DNT: 1
Host: 36.155.98.99
Origin: http://36.155.98.99
Pragma: no-cache
Referer: http://36.155.98.99/admin/
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36
X-Requested-With: XMLHttpRequest

这里是请求主体

mscId: 
```

如果content-type设置为application/x-www-form-urlencoded。则发送如下、这里面键值分别为ids和goods

```
ids=%20%5B%2260ee828f4ece8139f47c77b8%22%5D&key=%22goods%22
```

如果content-Type设置为 multipart/form-data; boundary=--------------------------553911955301029091719338这请求主体通过postman可以看到为下面的形式

```
----------------------------553911955301029091719338
Content-Disposition: form-data; name="ids"

["60ee828f4ece8139f47c77b8"]
----------------------------553911955301029091719338
Content-Disposition: form-data; name="key"

good
----------------------------553911955301029091719338--
```

#### （2）Content-Length 

Content-Length 是一个实体消息首部，用来指明发送给接收方的消息主体的大小（以字节为单位）

```js
Accept-Ranges: bytes
Access-Control-Allow-Credentials: true
Access-Control-Allow-Origin: http://localhost:3001
Access-Control-Expose-Headers: Date, Etag, Server, Connection, Accept-Ranges, Content-Range, Content-Encoding, Content-Length, Content-Type, Content-Disposition, Last-Modified, Content-Language, Cache-Control, Retry-After, X-Amz-Bucket-Region, Expires, X-Amz*, X-Amz*, *
Content-Length: 903539986  //单位为字节bytes
Content-Security-Policy: block-all-mixed-content
Content-Type: video/mp4
Date: Tue, 27 Jul 2021 07:26:07 GMT
ETag: "cbca8efdf8cbb4b524cf22c8110293be"
Last-Modified: Sat, 24 Jul 2021 17:05:39 GMT
Server: MinIO
Vary: Origin
X-Amz-Request-Id: 1695951741FE0A17
X-Xss-Protection: 1; mode=bloc
```

这里的大小是包含了所有内容编码的实体大小，如果对请求进行了gzip压缩的话，他指的进行了压缩后的大小，而不是原始的大小。还有就是除非使用了分块编码，否则Content-Length 首部是必须的。

（3）range与Content-Range

 Range 是一个请求首部，告知服务器返回文件的哪一部分。在一个  Range 首部中，可以一次性请求多个部分，服务器会以 multipart 文件的形式将其返回。如果服务器返回的是范围响应，需要使用 206 Partial Content 状态码。假如所请求的范围不合法，那么服务器会返回  416 Range Not Satisfiable 状态码，表示客户端错误。服务器允许忽略  Range  首部，从而返回整个文件，状态码用 200 。

```js
Range: <unit>=<range-start>-
Range: <unit>=<range-start>-<range-end>
Range: <unit>=<range-start>-<range-end>, <range-start>-<range-end>
Range: <unit>=<range-start>-<range-end>, <range-start>-<range-end>, <range-start>-<range-end>
```

在HTTP协议中，响应首部 Content-Range 显示的是一个数据片段在整个文件中的位置。

```js
Content-Range: <unit> <range-start>-<range-end>/<size>
Content-Range: <unit> <range-start>-<range-end>/*
Content-Range: <unit> */<size>
```

注意利用range和Content-Range我们可以实现大文件的分片下载

#### （3）Transfer-Encoding

`Transfer-Encoding` 响应头用于告诉客户端服务器发送内容的编码格式。

其可选值有：

- `chunked`：数据分块发送。此时应缺省 `Content-Length` 响应头。
- `compress`：使用 [Lempel-Ziv-Welch](http://en.wikipedia.org/wiki/LZW) 算法进行传输的格式，目前没有浏览器在支持。
- `deflate`：使用 [deflate](http://en.wikipedia.org/wiki/DEFLATE) 压缩算法 [zlib](http://en.wikipedia.org/wiki/Zlib) 结构。
- `gzip`：使用 [Lempel-Ziv coding](http://en.wikipedia.org/wiki/LZ77_and_LZ78#LZ77) 编码的压缩格式。
- `identity`：标识身份函数（e.g. no compression, nor modification）。

也可以同时指定多个值，用逗号分隔，像这样：`Transfer-Encoding: gzip, chunked`。

其中，`chunked` 就比较有意思了。它表示服务器下发到客户端的内容不是一次性完成的，而是分成一小块一小块（trunk）下发，过程中客户端与服务器的连接仍然维持不会断开。

在 Web Socket 没出来前，可利用这一机制实现长连接的效果。









