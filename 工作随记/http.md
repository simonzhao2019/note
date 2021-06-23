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

1、预检请求

“需预检的请求”要求必须首先使用 [`OPTIONS`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/OPTIONS)到服务器，以获知服务器是否允许该实际请求。"预检请求“的使用，可以避免跨域请求对服务器的用户数据产生未预期的影响。

大多数浏览器不支持针对于预检请求的重定向。如果一个预检请求发生了重定向，浏览器将报告错误：

2、http请求中的关键几个字段理解

（1）withCredentials：是否携带身份凭证

[`XMLHttpRequest`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest).org/en-US/docs/Web/API/Fetch_API) 与 CORS 的一个有趣的特性是，可以基于  [HTTP cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies) 和 HTTP 认证信息发送身份凭证。一般而言，对于跨源 [`XMLHttpRequest`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest)la.org/en-US/docs/Web/API/Fetch_API) 请求，浏览器**不会**发送身份凭证信息。如果要发送凭证信息，需要设置 `XMLHttpRequest `的某个特殊标志位。

简单来说，我们只有将withCredentials` 标志设置为 `true，才能服务器发送 Cookies（第三方）。还有就是如果服务器端的响应中未携带 `Access-Control-Allow-Credentials: true` ，浏览器将不会把响应内容返回给请求的发送者。

（2） Access-Control-Allow-Origin 

这里我们顺便说另外一个字段 Access-Control-Allow-Origin 的值为。响应头指定了该响应的资源是否被允许与给定的[origin](https://developer.mozilla.org/zh-CN/docs/Glossary/Origin)共享，“`*`”表示所有域都具有访问资源的权限。这里我们需要注意的是如果withCredentials设置为true，则Access-Control-Allow-Origin不能设置为通配符*，必须指定域名





