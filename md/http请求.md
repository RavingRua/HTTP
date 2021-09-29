# HTTP请求

HTTP协议（HyperText Transfer Protocol，超文本传输协议）是因特网上应用最为广泛的一种网络传输协议，所有的WWW文件都必须遵守这个标准。

HTTP是一个基于TCP/IP通信协议来传递数据（HTML 文件, 图片文件, 查询结果等）。

## 工作原理

HTTP协议工作于客户端-服务端架构上。浏览器作为HTTP客户端通过**URL**向HTTP服务端即WEB服务器**发送所有请求**。

Web服务器有：Apache服务器，IIS服务器（Internet Information Services）等。

Web服务器根据接收到的请求后，向客户端发送响应信息。

HTTP默认端口号为**80**，但是你也可以改为8080或者其他端口。

HTTPS默认端口号**443**。

**HTTP三点注意事项：**

+ HTTP是**无连接**：无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并**收到客户的应答后，即断开连接**。采用这种方式可以节省传输时间。
+ HTTP是**媒体独立的**：这意味着，只要客户端和服务器**知道如何处理的数据内容**，**任何类型的数据**都可以通过HTTP发送。客户端以及服务器指定使用适合的MIME-type内容类型。
+ HTTP是**无状态**：HTTP协议是无状态协议（RESTful的基本原则）。无状态是指协议**对于事务处理没有记忆能力**，URL的**上下文无关**（同一个URL和其他连接信息无关，是独立的），服务器不存储客户端信息。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。（Cookie、Session、RESTful的意义）

以下图表展示了HTTP协议通信流程[^1]：

![cgiarch](https://www.runoob.com/wp-content/uploads/2013/11/cgiarch.gif)

[^1]: CGI(Common Gateway Interface 通用网关接口) 是 HTTP 服务器与你的或其它机器上的程序进行“交谈”的一种工具，其程序须运行在网络服务器上。绝大多数的 CGI 程序被用来解释处理来自表单的输入信息，并在服务器产生相应的处理，或将相应的信息反馈给浏览器。CGI 程序使网页具有交互功能。 ↩

***

# HTTP 消息结构

HTTP是**基于客户端/服务端（C/S）的架构模型**，通过一个可靠的链接来交换信息，是一个无状态的**请求/响应**协议。

一个HTTP"客户端"是一个**应用程序**（Web**浏览器**或**其他任何客户端**），通过连接到服务器达到向服务器发送一个或多个HTTP的请求的目的。

一个HTTP"服务器"同样也是一个**应用程序**（通常是一个Web服务，如Apache Web服务器或IIS服务器等），通过接收客户端的请求并向客户端发送HTTP响应数据。

HTTP使用**统一资源标识符**（Uniform Resource Identifiers, URI）来传输数据和建立连接。

一旦建立连接后，数据消息就通过类似Internet邮件所使用的格式[RFC5322]和多用途Internet邮件扩展（MIME）[RFC2045]来传送。

------

## 客户端请求消息

客户端发送一个HTTP请求到服务器的请求消息包括以下格式：**请求行（request line）**、**请求头部（header）**、**空行**和**请求数据（请求体）**四个部分组成，下图给出了请求报文的一般格式。

![img](https://www.runoob.com/wp-content/uploads/2013/11/2012072810301161.png)
客户端请求（GET没有请求体)：

```http
GET /hello.txt HTTP/1.1
User-Agent: curl/7.16.3 libcurl/7.16.3 OpenSSL/0.9.7l zlib/1.2.3
Host: www.example.com
Accept-Language: en, mi
```

## 服务器响应消息

HTTP响应也由四个部分组成，分别是：**状态行**、**消息报头**、**空行**和**响应正文（响应体）**。

![img](https://www.runoob.com/wp-content/uploads/2013/11/httpmessage.jpg)

------
服务端响应（响应体内容来自hello.txt文件）:

```HTTP
HTTP/1.1 200 OK
Date: Mon, 27 Jul 2009 12:28:53 GMT
Server: Apache
Last-Modified: Wed, 22 Jul 2009 19:15:56 GMT
ETag: "34aa387-d-1568eb00"
Accept-Ranges: bytes
Content-Length: 51
Vary: Accept-Encoding
Content-Type: text/plain
```

输出结果：

```
Hello World! My payload includes a trailing CRLF.
```

***

# HTTP 请求方法

根据 HTTP 标准，HTTP 请求可以使用多种请求方法。

HTTP1.0 定义了三种请求方法： GET, POST 和 HEAD方法。

HTTP1.1 新增了六种请求方法：OPTIONS、PUT、PATCH、DELETE、TRACE 和 CONNECT 方法。

| 序号 | 方法       | 描述                                                         |
| :--- | :--------- | :----------------------------------------------------------- |
| 1    | **GET**    | 请求指定的页面信息，并返回实体主体。                         |
| 2    | **HEAD**   | 类似于 GET 请求，只不过返回的响应中**没有具体的内容**，**用于获取报头** |
| 3    | **POST**   | 向指定资源提交数据进行处理请求（例如**提交表单**或者**上传文件**）。**数据被包含在请求体中**。POST 请求可能会导致新的资源的**建立**和/或已有资源的**修改**。 |
| 4    | **PUT**    | 从客户端向服务器传送的数据取代指定的文档的内容（即**修改**）。 |
| 5    | **DELETE** | 请求服务器**删除**指定的页面（或服务器内容），可以有请求体。 |
| 6    | CONNECT    | HTTP/1.1 协议中预留给能够将连接改为**管道方式**[^2]的代理服务器。 |
| 7    | OPTIONS    | 允许客户端**查看服务器的性能**，返回服务器针对特定资源所支持的HTTP请求方法。 |
| 8    | TRACE      | 回显服务器收到的请求，主要用于**测试或诊断**。               |
| 9    | PATCH      | 是对 PUT 方法的补充，用来对已知资源进行**局部更新** 。       |

[^2]:管道（pipelining）机制：基于持久连接，管道机制允许客户端并行发送多个请求，而不需要客户端发送一个请求，服务器响应这个请求之后，才能发送下一个请求。

***

# HTTP 响应头信息

HTTP请求头提供了关于请求，响应或者其他的发送实体的信息。

在本章节中我们将具体来介绍HTTP响应头信息。

## 简单请求和复杂请求

### 跨域资源共享

**CORS** （Cross-Origin Resource Sharing，跨域资源共享）是一个系统，由W3C制定。它由一系列传输的[HTTP头](https://developer.mozilla.org/zh-CN/docs/Glossary/HTTP_header)组成，这些HTTP头决定浏览器是否阻止前端 JavaScript 代码获取跨域请求的响应。

[同源安全策略](https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy) 默认阻止“跨域”获取资源。但是 CORS 给了web服务器这样的权限，即服务器可以选择，允许跨域请求访问到它们的资源。

同源就是指，**域名**、**协议**、**端口**均为相同。解决跨域问题的方案有**JSONP（前端方案）**和**CORS（后端方案）**

### [CORS 头](https://developer.mozilla.org/zh-CN/docs/Glossary/CORS#cors_头)

+ [`Access-Control-Allow-Origin`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Allow-Origin)

  指示请求的资源能共享给哪些域。可以使用**通配符**。

+ [`Access-Control-Allow-Credentials`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Allow-Credentials)

  指示当请求的凭证标记为 true 时，是否响应该请求。

+ [`Access-Control-Allow-Headers`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Allow-Headers)

  用在对预请求的响应中，指示实际的请求中可以使用哪些 HTTP 头。可以使用**通配符**。

+ [`Access-Control-Allow-Methods`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Allow-Methods)

  指定对预请求的响应中，哪些 HTTP 方法允许访问请求的资源。可以使用**通配符**。

+ [`Access-Control-Expose-Headers`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Expose-Headers)

  指示哪些 HTTP 头的名称能在响应中列出。可以使用**通配符**。

+ [`Access-Control-Max-Age`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Max-Age)

  指示预请求的结果能被缓存多久。

+ [`Access-Control-Request-Headers`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Request-Headers)

  用于发起一个预请求，告知服务器正式请求会使用那些 HTTP 头。可以使用**通配符**。

+ [`Access-Control-Request-Method`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Request-Method)

  用于发起一个预请求，告知服务器正式请求会使用哪一种 [HTTP 请求方法](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods)
  可以使用**通配符**。

+ [`Origin`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Origin)

  指示获取资源的请求是从什么域发起的。可以是**IP地址**和**URL**。

### 简单请求

+ 方法只能是**GET**、**POST**、**HEAD**
+ 允许的请求头：Accept、Accept-Language、Content-Language、Last-Event-ID、Content-Type，其中Content-Type只支持：
  + application/x-www-form-urlencoded（一般用来发送**基本表单**内容）
  + multipart/form-data（**发送文件**时需要的请求头，代表请求体内有多种类型）
  + text/plain（普通文本信息）

### 复杂请求

不满足简单请求要求的请求即是复杂请求。

## HTTP请求头

| 应答头                      | 说明                                                         |
| :-------------------------- | :----------------------------------------------------------- |
| Allow                       | 服务器支持哪些请求方法（如GET、POST等）。                    |
| Access-Control-Allow-Origin | 响应头指定了该响应的资源是否被允许与给定的[origin](https://developer.mozilla.org/zh-CN/docs/Glossary/Origin)共享。Orign即是**“源”**。将值设为'*'可以允许所有类型的响应，即允许跨域发送请求。默认下浏览器执行同源安全策略，不允许跨域请求，当一个请求的协议（http、https）不同、域（www.baidu.com和image.baidu.com）不同、端口不同时就是跨域行为。 |
| Content-Encoding            | 文档的编码（Encode）方法。只有在解码之后才可以得到Content-Type头指定的内容类型。利用gzip压缩文档能够显著地减少HTML文档的下载时间。Java的GZIPOutputStream可以很方便地进行gzip压缩，但只有Unix上的Netscape和Windows上的IE 4、IE 5才支持它。因此，Servlet应该通过查看Accept-Encoding头（即request.getHeader("Accept-Encoding")）检查浏览器是否支持gzip，为支持gzip的浏览器返回经gzip压缩的HTML页面，为其他浏览器返回普通页面。通常使用**UTF-8**。 |
| Content-Length              | 表示内容长度。只有当浏览器使用持久HTTP连接时才需要这个数据。如果你想要利用持久连接的优势，可以把输出文档写入 ByteArrayOutputStream，完成后查看其大小，然后把该值放入Content-Length头，最后通过byteArrayStream.writeTo(response.getOutputStream())发送内容。 |
| Content-Type                | 表示后面的文档属于什么MIME类型。Servlet（Java编写的服务器端程序）**默认为text/plain**，**但通常需要显式地指定为text/html**。由于经常要设置Content-Type，因此HttpServletResponse提供了一个专用的方法setContentType。 |
| Date                        | 当前的GMT时间。你可以用setDateHeader来设置这个头以避免转换时间格式的麻烦。 |
| Expires                     | 应该在什么时候认为文档已经过期，从而不再缓存它？             |
| Last-Modified               | 文档的最后改动时间。客户可以通过If-Modified-Since请求头提供一个日期，该请求将被视为一个条件GET，只有改动时间迟于指定时间的文档才会返回，否则返回一个304（Not Modified）状态。Last-Modified也可用setDateHeader方法来设置。 |
| Location                    | 表示客户应当到哪里去提取文档。Location通常不是直接设置的，而是通过HttpServletResponse的sendRedirect方法，该方法同时设置状态代码为302（请求的资源现在临时从不同的 URI 响应请求）。 |
| Refresh                     | 表示浏览器应该在多少时间之后刷新文档，以秒计。除了刷新当前文档之外，你还可以通过setHeader("Refresh", "5; URL=http://host/path")让浏览器读取指定的页面。 注意这种功能通常是通过设置HTML页面HEAD区的＜META HTTP-EQUIV="Refresh" CONTENT="5;URL=http://host/path"＞实现，这是因为，自动刷新或重定向对于那些不能使用CGI或Servlet的HTML编写者十分重要。但是，对于Servlet来说，直接设置Refresh头更加方便。  注意Refresh的意义是"N秒之后刷新本页面或访问指定页面"，而不是"每隔N秒刷新本页面或访问指定页面"。因此，连续刷新要求每次都发送一个Refresh头，而发送204状态代码则可以阻止浏览器继续刷新，不管是使用Refresh头还是＜META HTTP-EQUIV="Refresh" ...＞。  注意Refresh头不属于HTTP 1.1正式规范的一部分，而是一个扩展，但Netscape和IE都支持它。 |
| Server                      | 服务器名字。Servlet一般不设置这个值，而是由Web服务器自己设置。 |
| Set-Cookie                  | 设置和页面关联的Cookie。Servlet不应使用response.setHeader("Set-Cookie", ...)，而是应使用HttpServletResponse提供的专用方法addCookie。参见下文有关Cookie设置的讨论。 |
| WWW-Authenticate            | 客户应该在Authorization头中提供什么类型的授权信息？**在包含401（Unauthorized）状态行的应答中这个头是必需的。**例如，response.setHeader("WWW-Authenticate", "BASIC realm=＼"executives＼"")。 注意Servlet一般不进行这方面的处理，而是让Web服务器的专门机制来控制受密码保护页面的访问（例如.htaccess）。 |

***

# HTTP content-type

Content-Type（内容类型），一般是指网页中存在的 Content-Type，用于定义网络文件的类型和网页的编码，决定浏览器将以什么形**式**（媒体类型）、什么**编码**读取这个文件，这就是经常看到一些 PHP 网页点击的结果却是下载一个文件或一张图片的原因。

Content-Type 标头告诉客户端实际返回的内容的内容类型。

语法格式：

```
Content-Type: text/html; charset=utf-8
Content-Type: multipart/form-data; boundary=something
```

实例：

![img](https://www.runoob.com/wp-content/uploads/2014/06/F7E193D6-3C08-4B97-BAF2-FF340DAA5C6E.jpg)

常见的媒体格式类型如下：

+ text/html ： HTML格式
+ text/plain ：纯文本格式
+ text/xml ： XML格式
+ image/gif ：gif图片格式
+ image/jpeg ：jpg图片格式
+ image/png：png图片格式

以application开头的媒体格式类型：

+ application/xhtml+xml ：XHTML格式
+ application/xml： XML数据格式
+ application/atom+xml ：Atom XML聚合格式
+ application/json： JSON数据格式
+ application/pdf：pdf格式
+ application/msword ： Word文档格式
+ application/octet-stream ： 二进制流数据（如常见的文件下载）
+ application/x-www-form-urlencoded ： ```html<form encType="">```中默认的encType，form表单数据被编码为**key/value**格式发送到服务器（表单**默认**的提交数据的格式）

另外一种常见的媒体格式是上传文件之时使用的：

+ multipart/form-data ： 需要在表单中**进行文件上传**时，就需要使用该格式