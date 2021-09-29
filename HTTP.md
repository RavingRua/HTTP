# HTTP

---

> 前置：计算机网络 [Computer Networking](https://github.com/RavingRua/Computer-Networking)

## 第一章 HTTP 概述

### 1.1 Web 基础架构

Web 选择了经典的**客户端-浏览器架构（client-server architecture）**作为其运行架构。HTTP 客户端（如 Web 浏览器）进程通过客户端端系统的套接字发送报文到服务器端套接字，并由服务器端进程接收报文并返回响应。

### 1.2 资源

Web 服务器是 **Web 资源（Web resource）**的宿主，Web 资源是 Web 内容的源头。

最简单的资源是一些**静态文件**，如文本文件、HTML 文件、图片、视频、文档等。资源还可以是由其他应用进程（如模板引擎）生成的内容，这些**动态资源**可以根据客户端身份、请求信息、时间等因素动态生成。

#### 1.2.1 媒体类型

由于因特网需要传输的数据众多，HTTP 为每种需要通过 Web 传输的对象都打上了 **MIME 类型（MIME type）**的数据格式标签。

MIME，即 **Multipurpose Internet Mail Extensions 多用途互联网邮件扩展类型**，最早用于解决不同电子邮件系统间迁移报文时存在的问题。HTTP 使用 MIME 来标记超媒体内容。Web 服务器会在响应报文的`Content-type:`首部行中包含 MIME 类型的值，指示浏览器或其他客户端进程如何解析实体体中封装的内容。

MIME 类型是一种文本标记，详细内容见附录 [HTTP content-type 对照表](#content-type) 。

#### 1.2.2 URI

每一个 Web 资源都有一个名字，这个名字被称为 **URI（Uniform Resource Identifier，统一资源标识符）**，该名字表示了世界范围**互联网**中一份资源的。如 Github 中的一份 md 文件的 URI 为：

```uri
https://github.com/RavingRua/HTTP/blob/master/HTTP.md
```

URI 有两种形式：URL 和 URN 。

#### 1.2.3 URL

**统一资源定位器（Uniform Resource Locator）**是 URI 最常见的形式。URL 描述了一台**特定服务器**上某资源的位置。上方资源的 URI 也是其 URL，URL 通常遵循以下格式：

```url
protocol://hostname[:port]/path[;parameters][?query]#fragment
```

+ 第一部分为**方案（scheme）**，说明访问资源所用的协议类型
+ 第二部分为**因特网地址**，通常是域名或 IP 地址，后面可以跟上一个可选的**端口**
+ 第三部分为**地址（path）**，指定了服务器上的某个资源
+ 剩下部分为可选的**参数**、**查询字符串**和**信息碎片（如文档锚点）**

现在几乎所有的 URI 都是 URL 。

#### 1.2.4 URN

**统一资源名（Uniform Resource Name）**是特定资源的唯一名称，但与资源目前所在地无关，因此可以随意搬运并仍然可被定位。URN 还处于试验阶段，未大规模使用。

### 1.3 事务

HTTP 通过两种类型的应用层报文（message）：**请求**和**响应**来处理。

#### 1.3.1 方法

HTTP 有一些不同的请求命令，称为**方法（method）**，每条请求报文的请求行都包括方法的字段，这些命令会告诉服务器执行什么动作。以下是常用 HTTP 方法：

| 序号 | 方法       | 描述                                                         |
| :--- | :--------- | :----------------------------------------------------------- |
| 1    | **GET**    | 请求指定的页面信息，并返回实体主体。                         |
| 2    | **HEAD**   | 类似于 GET 请求，只不过返回的响应中**没有具体的内容**，**用于获取报头** |
| 3    | **POST**   | 向指定资源提交数据进行处理请求（例如**提交表单**或者**上传文件**）。**数据被包含在请求体中**。POST 请求可能会导致新的资源的**建立**和/或已有资源的**修改**。 |
| 4    | **PUT**    | 从客户端向服务器传送的数据取代指定的文档的内容（即**修改**）。 |
| 5    | **DELETE** | 请求服务器**删除**指定的页面（或服务器内容），可以有请求体。 |

#### 1.3.2 状态码

每条 HTTP 响应报文的响应行中会包含一个**状态码（status code）**字段，告知客户端是否响应成功，以及应该进行何种操作。每个响应码字段后还会包含一个**响应消息**字段，以便人能够快速理解。

所有响应状态码见附录 [HTTP status code](#status-code) 。

#### 1.3.3 一个 Web 页面可以包含多个对象

现代 Web 页面，尤其是复杂的 Web 应用中，可能会包含多个资源对象，或包含多项 Web 事务。客户端会在初始化页面或需要使用时发送 HTTP 请求获取需要的资源。

### 1.4 报文

一则 HTTP 报文通常包含以下结构：

+ 起始行：请求报文中为请求行，包括请求方法、URL、HTTP 协议版本字段；响应报文中为状态行，包含 HTTP 协议版本、状态码、状态消息字段
+ 首部行：起始行后有一个或多个首部字段，用于指示客户端或浏览器如何解析报文内容
+ 空行：和其他行一样，空行使用 CRLF（回车换行）进行换行，改行没有任何内容
+ 实体体：实体体封装了报文发送的其他内容

### 1.5 连接

HTTP 是 OSI 七层模型中的应用层报文，基于运输层的 TCP/IP 协议。

#### 1.5.1 TCP

TCP 向 HTTP 提供了以下服务：

+ 面向连接服务：为上层建立和维护可靠持续连接
+ 可靠数据传输服务：保证无差错、按适当顺序传输数据
+ 未分段数据流服务：可以在任意时刻发送任意大小分组

#### 1.5.2 连接、IP 地址和端口号

HTTP 通过端系统套接字发送报文前，套接字会先与服务器套接字建立一条 TCP 连接，并使用 **IP**（Internet Protocol）地址告知网络层寻找服务器位置。端口号将指示服务器端套接字寻找到对应的进程。

### 1.6 协议版本

HTTP 有几个版本，目前使用最多的是 HTTP 1.1，HTTP 2.0 仍在推广和试验。

+ HTTP/0.9：这是 1991 年欧洲粒子物理研究所的 TimBL 和其团队实现的 HTTP 原型版本，这个版本的 HTTP 只支持 GET 方法，不支持超媒体内容类型、首部行、版本号等；
+ HTTP/1.0：第一个广泛使用的版本，添加了版本号、首部、额外方法，以及 MIME 类型；
+ HTTP/1.0+：Web 在推出后就得到各方支持并被快速推广，因此也诞生了不少非标准协议，这些协议统称 HTTP/1.0+，包括持续连接 keep-alive 就是在该时期被提出的；
+ HTTP/1.1：目前使用最广的协议版本，校正了1.0版本中的结构性缺陷，明确语义，并引入新的方法；
+ HTTP-NG（HTTP/2.0）：Next Generation 版本的 HTTP 的关注重点是性能优化以及更强大的服务逻辑远程执行框架。关于这个版本协议的研究工作在1998年就已经结束，在2012年再度被 HTTP 工作小组推上日程。该协议的实现和推广还需一段时间。

### 1.7 Web 结构组件

Web 浏览器和 Web 服务器是整个 Web 栈中最重要的两个部分，任何在线 Web 应用都必须包含这两个结构组件。此外，Web 栈中还包含一下应用：

+ 代理：位于客户端和真实服务器之间的中间实体
+ 缓存：Web 的中间仓库
+ 网关：连接其他应用的特殊 Web 服务器
+ 隧道：对 HTTP 报文进行盲转发的特殊代理
+ Agent 代理：可以自动发起请求的半智能客户端

#### 1.7.1 代理

**代理（proxy）**位于客户端与服务器中间，接收所有客户端的 HTTP 请求，并将请求转发给服务器。这一过程中请求可能会被修改。出于安全考虑，现在的大型 Web 应用都会使用代理。

#### 1.7.2 缓存

**Web 缓存（Web cache）**也称代理缓存（proxy cache），是一种特殊的代理服务器。可以通过缓存将一些常用的资源保存起来，下次访问这些资源可以直接从缓存中获取。缓存可能是一个单独的中间代理服务器，也可能是客户端进程的一部分。

#### 1.7.3 网关

**网关（gateway）**是一种特殊的服务器，，作为其他服务器的中间实体使用，通常用于将 HTTP 协议的流量转换为其他协议进行运输。对于客户端来说，网关就像一个服务器端系统一样，有时客户端可能不知道自己实际上在与网关通信。

#### 1.7.4 隧道

**隧道（tunnel）**是在 TCP 连接建立后就会在两条连接之间对原始数据进行盲转发的 HTTP 应用程序。HTTP 隧道通常用来在一条或多条 HTTP 持续连接上转发非 HTTP 数据。本质上就是把其他协议的报文封装在 HTTP 报文中。

HTTP 隧道其中的一种用处是通过 HTTP 承载 SSL（安全套接字层）的流量，这样这些流量就可以通过只允许 Web 流量进入的防火墙：

在 HTTPS 中，报文首部信息会使用最终服务器的公钥对报文进行加密，而代理服务器无法理解加密的首部信息，因此无法进行转发。因此，在进行 HTTPS 报文转发前，会有一条 CONNECT 方法的报文发向代理服务器，告知代理服务器建立客户端到真实服务器的连接，随后 HTTPS 隧道会封装 SSL 信息并发送给服务器进行解密验证，而代理服务器因为没有解密私钥，无法获得封装在隧道内的 SSL 信息，因此保证了 HTTPS 的安全。

#### 1.7.5 Agent 代理

**User-Agent 代理**是代表用户发起 HTTP 请求的客户端进程。Web 浏览器是用户最常见的 Agent 类型，开发人员可能会使用其他调试软件发送 HTTP 请求。此外，网络蜘蛛和网络爬虫也是一类 Agent 。

---

## 附录

### HTTP content-type 对照表

<span id="content-type"></span>

| 文件扩展名                          | Content-Type(Mime-Type)                 | 文件扩展名 | Content-Type(Mime-Type)             |
| :---------------------------------- | :-------------------------------------- | :--------- | :---------------------------------- |
| .*（ 二进制流，不知道下载文件类型） | application/octet-stream                | .tif       | image/tiff                          |
| .001                                | application/x-001                       | .301       | application/x-301                   |
| .323                                | text/h323                               | .906       | application/x-906                   |
| .907                                | drawing/907                             | .a11       | application/x-a11                   |
| .acp                                | audio/x-mei-aac                         | .ai        | application/postscript              |
| .aif                                | audio/aiff                              | .aifc      | audio/aiff                          |
| .aiff                               | audio/aiff                              | .anv       | application/x-anv                   |
| .asa                                | text/asa                                | .asf       | video/x-ms-asf                      |
| .asp                                | text/asp                                | .asx       | video/x-ms-asf                      |
| .au                                 | audio/basic                             | .avi       | video/avi                           |
| .awf                                | application/vnd.adobe.workflow          | .biz       | text/xml                            |
| .bmp                                | application/x-bmp                       | .bot       | application/x-bot                   |
| .c4t                                | application/x-c4t                       | .c90       | application/x-c90                   |
| .cal                                | application/x-cals                      | .cat       | application/vnd.ms-pki.seccat       |
| .cdf                                | application/x-netcdf                    | .cdr       | application/x-cdr                   |
| .cel                                | application/x-cel                       | .cer       | application/x-x509-ca-cert          |
| .cg4                                | application/x-g4                        | .cgm       | application/x-cgm                   |
| .cit                                | application/x-cit                       | .class     | java/*                              |
| .cml                                | text/xml                                | .cmp       | application/x-cmp                   |
| .cmx                                | application/x-cmx                       | .cot       | application/x-cot                   |
| .crl                                | application/pkix-crl                    | .crt       | application/x-x509-ca-cert          |
| .csi                                | application/x-csi                       | .css       | text/css                            |
| .cut                                | application/x-cut                       | .dbf       | application/x-dbf                   |
| .dbm                                | application/x-dbm                       | .dbx       | application/x-dbx                   |
| .dcd                                | text/xml                                | .dcx       | application/x-dcx                   |
| .der                                | application/x-x509-ca-cert              | .dgn       | application/x-dgn                   |
| .dib                                | application/x-dib                       | .dll       | application/x-msdownload            |
| .doc                                | application/msword                      | .dot       | application/msword                  |
| .drw                                | application/x-drw                       | .dtd       | text/xml                            |
| .dwf                                | Model/vnd.dwf                           | .dwf       | application/x-dwf                   |
| .dwg                                | application/x-dwg                       | .dxb       | application/x-dxb                   |
| .dxf                                | application/x-dxf                       | .edn       | application/vnd.adobe.edn           |
| .emf                                | application/x-emf                       | .eml       | message/rfc822                      |
| .ent                                | text/xml                                | .epi       | application/x-epi                   |
| .eps                                | application/x-ps                        | .eps       | application/postscript              |
| .etd                                | application/x-ebx                       | .exe       | application/x-msdownload            |
| .fax                                | image/fax                               | .fdf       | application/vnd.fdf                 |
| .fif                                | application/fractals                    | .fo        | text/xml                            |
| .frm                                | application/x-frm                       | .g4        | application/x-g4                    |
| .gbr                                | application/x-gbr                       | .          | application/x-                      |
| .gif                                | image/gif                               | .gl2       | application/x-gl2                   |
| .gp4                                | application/x-gp4                       | .hgl       | application/x-hgl                   |
| .hmr                                | application/x-hmr                       | .hpg       | application/x-hpgl                  |
| .hpl                                | application/x-hpl                       | .hqx       | application/mac-binhex40            |
| .hrf                                | application/x-hrf                       | .hta       | application/hta                     |
| .htc                                | text/x-component                        | .htm       | text/html                           |
| .html                               | text/html                               | .htt       | text/webviewhtml                    |
| .htx                                | text/html                               | .icb       | application/x-icb                   |
| .ico                                | image/x-icon                            | .ico       | application/x-ico                   |
| .iff                                | application/x-iff                       | .ig4       | application/x-g4                    |
| .igs                                | application/x-igs                       | .iii       | application/x-iphone                |
| .img                                | application/x-img                       | .ins       | application/x-internet-signup       |
| .isp                                | application/x-internet-signup           | .IVF       | video/x-ivf                         |
| .java                               | java/*                                  | .jfif      | image/jpeg                          |
| .jpe                                | image/jpeg                              | .jpe       | application/x-jpe                   |
| .jpeg                               | image/jpeg                              | .jpg       | image/jpeg                          |
| .jpg                                | application/x-jpg                       | .js        | application/x-javascript            |
| .jsp                                | text/html                               | .la1       | audio/x-liquid-file                 |
| .lar                                | application/x-laplayer-reg              | .latex     | application/x-latex                 |
| .lavs                               | audio/x-liquid-secure                   | .lbm       | application/x-lbm                   |
| .lmsff                              | audio/x-la-lms                          | .ls        | application/x-javascript            |
| .ltr                                | application/x-ltr                       | .m1v       | video/x-mpeg                        |
| .m2v                                | video/x-mpeg                            | .m3u       | audio/mpegurl                       |
| .m4e                                | video/mpeg4                             | .mac       | application/x-mac                   |
| .man                                | application/x-troff-man                 | .math      | text/xml                            |
| .mdb                                | application/msaccess                    | .mdb       | application/x-mdb                   |
| .mfp                                | application/x-shockwave-flash           | .mht       | message/rfc822                      |
| .mhtml                              | message/rfc822                          | .mi        | application/x-mi                    |
| .mid                                | audio/mid                               | .midi      | audio/mid                           |
| .mil                                | application/x-mil                       | .mml       | text/xml                            |
| .mnd                                | audio/x-musicnet-download               | .mns       | audio/x-musicnet-stream             |
| .mocha                              | application/x-javascript                | .movie     | video/x-sgi-movie                   |
| .mp1                                | audio/mp1                               | .mp2       | audio/mp2                           |
| .mp2v                               | video/mpeg                              | .mp3       | audio/mp3                           |
| .mp4                                | video/mpeg4                             | .mpa       | video/x-mpg                         |
| .mpd                                | application/vnd.ms-project              | .mpe       | video/x-mpeg                        |
| .mpeg                               | video/mpg                               | .mpg       | video/mpg                           |
| .mpga                               | audio/rn-mpeg                           | .mpp       | application/vnd.ms-project          |
| .mps                                | video/x-mpeg                            | .mpt       | application/vnd.ms-project          |
| .mpv                                | video/mpg                               | .mpv2      | video/mpeg                          |
| .mpw                                | application/vnd.ms-project              | .mpx       | application/vnd.ms-project          |
| .mtx                                | text/xml                                | .mxp       | application/x-mmxp                  |
| .net                                | image/pnetvue                           | .nrf       | application/x-nrf                   |
| .nws                                | message/rfc822                          | .odc       | text/x-ms-odc                       |
| .out                                | application/x-out                       | .p10       | application/pkcs10                  |
| .p12                                | application/x-pkcs12                    | .p7b       | application/x-pkcs7-certificates    |
| .p7c                                | application/pkcs7-mime                  | .p7m       | application/pkcs7-mime              |
| .p7r                                | application/x-pkcs7-certreqresp         | .p7s       | application/pkcs7-signature         |
| .pc5                                | application/x-pc5                       | .pci       | application/x-pci                   |
| .pcl                                | application/x-pcl                       | .pcx       | application/x-pcx                   |
| .pdf                                | application/pdf                         | .pdf       | application/pdf                     |
| .pdx                                | application/vnd.adobe.pdx               | .pfx       | application/x-pkcs12                |
| .pgl                                | application/x-pgl                       | .pic       | application/x-pic                   |
| .pko                                | application/vnd.ms-pki.pko              | .pl        | application/x-perl                  |
| .plg                                | text/html                               | .pls       | audio/scpls                         |
| .plt                                | application/x-plt                       | .png       | image/png                           |
| .png                                | application/x-png                       | .pot       | application/vnd.ms-powerpoint       |
| .ppa                                | application/vnd.ms-powerpoint           | .ppm       | application/x-ppm                   |
| .pps                                | application/vnd.ms-powerpoint           | .ppt       | application/vnd.ms-powerpoint       |
| .ppt                                | application/x-ppt                       | .pr        | application/x-pr                    |
| .prf                                | application/pics-rules                  | .prn       | application/x-prn                   |
| .prt                                | application/x-prt                       | .ps        | application/x-ps                    |
| .ps                                 | application/postscript                  | .ptn       | application/x-ptn                   |
| .pwz                                | application/vnd.ms-powerpoint           | .r3t       | text/vnd.rn-realtext3d              |
| .ra                                 | audio/vnd.rn-realaudio                  | .ram       | audio/x-pn-realaudio                |
| .ras                                | application/x-ras                       | .rat       | application/rat-file                |
| .rdf                                | text/xml                                | .rec       | application/vnd.rn-recording        |
| .red                                | application/x-red                       | .rgb       | application/x-rgb                   |
| .rjs                                | application/vnd.rn-realsystem-rjs       | .rjt       | application/vnd.rn-realsystem-rjt   |
| .rlc                                | application/x-rlc                       | .rle       | application/x-rle                   |
| .rm                                 | application/vnd.rn-realmedia            | .rmf       | application/vnd.adobe.rmf           |
| .rmi                                | audio/mid                               | .rmj       | application/vnd.rn-realsystem-rmj   |
| .rmm                                | audio/x-pn-realaudio                    | .rmp       | application/vnd.rn-rn_music_package |
| .rms                                | application/vnd.rn-realmedia-secure     | .rmvb      | application/vnd.rn-realmedia-vbr    |
| .rmx                                | application/vnd.rn-realsystem-rmx       | .rnx       | application/vnd.rn-realplayer       |
| .rp                                 | image/vnd.rn-realpix                    | .rpm       | audio/x-pn-realaudio-plugin         |
| .rsml                               | application/vnd.rn-rsml                 | .rt        | text/vnd.rn-realtext                |
| .rtf                                | application/msword                      | .rtf       | application/x-rtf                   |
| .rv                                 | video/vnd.rn-realvideo                  | .sam       | application/x-sam                   |
| .sat                                | application/x-sat                       | .sdp       | application/sdp                     |
| .sdw                                | application/x-sdw                       | .sit       | application/x-stuffit               |
| .slb                                | application/x-slb                       | .sld       | application/x-sld                   |
| .slk                                | drawing/x-slk                           | .smi       | application/smil                    |
| .smil                               | application/smil                        | .smk       | application/x-smk                   |
| .snd                                | audio/basic                             | .sol       | text/plain                          |
| .sor                                | text/plain                              | .spc       | application/x-pkcs7-certificates    |
| .spl                                | application/futuresplash                | .spp       | text/xml                            |
| .ssm                                | application/streamingmedia              | .sst       | application/vnd.ms-pki.certstore    |
| .stl                                | application/vnd.ms-pki.stl              | .stm       | text/html                           |
| .sty                                | application/x-sty                       | .svg       | text/xml                            |
| .swf                                | application/x-shockwave-flash           | .tdf       | application/x-tdf                   |
| .tg4                                | application/x-tg4                       | .tga       | application/x-tga                   |
| .tif                                | image/tiff                              | .tif       | application/x-tif                   |
| .tiff                               | image/tiff                              | .tld       | text/xml                            |
| .top                                | drawing/x-top                           | .torrent   | application/x-bittorrent            |
| .tsd                                | text/xml                                | .txt       | text/plain                          |
| .uin                                | application/x-icq                       | .uls       | text/iuls                           |
| .vcf                                | text/x-vcard                            | .vda       | application/x-vda                   |
| .vdx                                | application/vnd.visio                   | .vml       | text/xml                            |
| .vpg                                | application/x-vpeg005                   | .vsd       | application/vnd.visio               |
| .vsd                                | application/x-vsd                       | .vss       | application/vnd.visio               |
| .vst                                | application/vnd.visio                   | .vst       | application/x-vst                   |
| .vsw                                | application/vnd.visio                   | .vsx       | application/vnd.visio               |
| .vtx                                | application/vnd.visio                   | .vxml      | text/xml                            |
| .wav                                | audio/wav                               | .wax       | audio/x-ms-wax                      |
| .wb1                                | application/x-wb1                       | .wb2       | application/x-wb2                   |
| .wb3                                | application/x-wb3                       | .wbmp      | image/vnd.wap.wbmp                  |
| .wiz                                | application/msword                      | .wk3       | application/x-wk3                   |
| .wk4                                | application/x-wk4                       | .wkq       | application/x-wkq                   |
| .wks                                | application/x-wks                       | .wm        | video/x-ms-wm                       |
| .wma                                | audio/x-ms-wma                          | .wmd       | application/x-ms-wmd                |
| .wmf                                | application/x-wmf                       | .wml       | text/vnd.wap.wml                    |
| .wmv                                | video/x-ms-wmv                          | .wmx       | video/x-ms-wmx                      |
| .wmz                                | application/x-ms-wmz                    | .wp6       | application/x-wp6                   |
| .wpd                                | application/x-wpd                       | .wpg       | application/x-wpg                   |
| .wpl                                | application/vnd.ms-wpl                  | .wq1       | application/x-wq1                   |
| .wr1                                | application/x-wr1                       | .wri       | application/x-wri                   |
| .wrk                                | application/x-wrk                       | .ws        | application/x-ws                    |
| .ws2                                | application/x-ws                        | .wsc       | text/scriptlet                      |
| .wsdl                               | text/xml                                | .wvx       | video/x-ms-wvx                      |
| .xdp                                | application/vnd.adobe.xdp               | .xdr       | text/xml                            |
| .xfd                                | application/vnd.adobe.xfd               | .xfdf      | application/vnd.adobe.xfdf          |
| .xhtml                              | text/html                               | .xls       | application/vnd.ms-excel            |
| .xls                                | application/x-xls                       | .xlw       | application/x-xlw                   |
| .xml                                | text/xml                                | .xpl       | audio/scpls                         |
| .xq                                 | text/xml                                | .xql       | text/xml                            |
| .xquery                             | text/xml                                | .xsd       | text/xml                            |
| .xsl                                | text/xml                                | .xslt      | text/xml                            |
| .xwd                                | application/x-xwd                       | .x_b       | application/x-x_b                   |
| .sis                                | application/vnd.symbian.install         | .sisx      | application/vnd.symbian.install     |
| .x_t                                | application/x-x_t                       | .ipa       | application/vnd.iphone              |
| .apk                                | application/vnd.android.package-archive | .xap       | application/x-silverlight-app       |

### HTTP status code

<span id="status-code"></span>

100 Continue

这个临时响应表明，迄今为止的所有内容都是可行的，客户端应该继续请求，如果已经完成，则忽略它。

101 Switching Protocol

该代码是响应客户端的 Upgrade (en-US) 标头发送的，并且指示服务器也正在切换的协议。

102 Processing (WebDAV)

此代码表示服务器已收到并正在处理该请求，但没有响应可用。

103 Early Hints

此状态代码主要用于与Link 链接头一起使用，以允许用户代理在服务器仍在准备响应时开始预加载资源。
成功响应

200 OK

请求成功。成功的含义取决于HTTP方法：

GET：资源已被提取并在消息正文中传输。
HEAD：实体标头位于消息正文中。
POST：描述动作结果的资源在消息体中传输。
TRACE：消息正文包含服务器收到的请求消息

201 Created

该请求已成功，并因此创建了一个新的资源。这通常是在POST请求，或是某些PUT请求之后返回的响应。

202 Accepted

请求已经接收到，但还未响应，没有结果。意味着不会有一个异步的响应去表明当前请求的结果，预期另外的进程和服务去处理请求，或者批处理。

203 Non-Authoritative Information

服务器已成功处理了请求，但返回的实体头部元信息不是在原始服务器上有效的确定集合，而是来自本地或者第三方的拷贝。当前的信息可能是原始版本的子集或者超集。例如，包含资源的元数据可能导致原始服务器知道元信息的超集。使用此状态码不是必须的，而且只有在响应不使用此状态码便会返回200 OK的情况下才是合适的。

204 No Content

服务器成功处理了请求，但不需要返回任何实体内容，并且希望返回更新了的元信息。响应可能通过实体头部的形式，返回新的或更新后的元信息。如果存在这些头部信息，则应当与所请求的变量相呼应。如果客户端是浏览器的话，那么用户浏览器应保留发送了该请求的页面，而不产生任何文档视图上的变化，即使按照规范新的或更新后的元信息应当被应用到用户浏览器活动视图中的文档。由于204响应被禁止包含任何消息体，因此它始终以消息头后的第一个空行结尾。

205 Reset Content

服务器成功处理了请求，且没有返回任何内容。但是与204响应不同，返回此状态码的响应要求请求者重置文档视图。该响应主要是被用于接受用户输入后，立即重置表单，以便用户能够轻松地开始另一次输入。与204响应一样，该响应也被禁止包含任何消息体，且以消息头后的第一个空行结束。

206 Partial Content

服务器已经成功处理了部分 GET 请求。类似于 FlashGet 或者迅雷这类的 HTTP 下载工具都是使用此类响应实现断点续传或者将一个大文档分解为多个下载段同时下载。该请求必须包含 Range 头信息来指示客户端希望得到的内容范围，并且可能包含 If-Range 来作为请求条件。

207 Multi-Status (WebDAV)

由WebDAV(RFC 2518)扩展的状态码，代表之后的消息体将是一个XML消息，并且可能依照之前子请求数量的不同，包含一系列独立的响应代码。

208 Already Reported (WebDAV)

在 DAV 里面使用: propstat 响应元素以避免重复枚举多个绑定的内部成员到同一个集合。

226 IM Used (HTTP Delta encoding)

服务器已经完成了对资源的 GET 请求，并且响应是对当前实例应用的一个或多个实例操作结果的表示。
重定向

300 Multiple Choice

被请求的资源有一系列可供选择的回馈信息，每个都有自己特定的地址和浏览器驱动的商议信息。用户或浏览器能够自行选择一个首选的地址进行重定向。

301 Moved Permanently

被请求的资源已永久移动到新位置，并且将来任何对此资源的引用都应该使用本响应返回的若干个 URI 之一。如果可能，拥有链接编辑功能的客户端应当自动把请求的地址修改为从服务器反馈回来的地址。除非额外指定，否则这个响应也是可缓存的。

302 Found

请求的资源现在临时从不同的 URI 响应请求。由于这样的重定向是临时的，客户端应当继续向原有地址发送以后的请求。只有在Cache-Control或Expires中进行了指定的情况下，这个响应才是可缓存的。

303 See Other

对应当前请求的响应可以在另一个 URI 上被找到，而且客户端应当采用 GET 的方式访问那个资源。这个方法的存在主要是为了允许由脚本激活的POST请求输出重定向到一个新的资源。

304 Not Modified

如果客户端发送了一个带条件的 GET 请求且该请求已被允许，而文档的内容（自上次访问以来或者根据请求的条件）并没有改变，则服务器应当返回这个状态码。304 响应禁止包含消息体，因此始终以消息头后的第一个空行结尾。

305 Use Proxy

被请求的资源必须通过指定的代理才能被访问。Location 域中将给出指定的代理所在的 URI 信息，接收者需要重复发送一个单独的请求，通过这个代理才能访问相应资源。只有原始服务器才能建立305响应。

306 unused

在最新版的规范中，306 状态码已经不再被使用。

307 Temporary Redirect

请求的资源现在临时从不同的URI 响应请求。由于这样的重定向是临时的，客户端应当继续向原有地址发送以后的请求。只有在Cache-Control或Expires中进行了指定的情况下，这个响应才是可缓存的。

308 Permanent Redirect

这意味着资源现在永久位于由 Location: HTTP Response 标头指定的另一个 URI。 这与 301 Moved Permanently HTTP 响应代码具有相同的语义，但用户代理不能更改所使用的 HTTP 方法：如果在第一个请求中使用 POST，则必须在第二个请求中使用 POST。
客户端响应

400 Bad Request

1、语义有误，当前请求无法被服务器理解。除非进行修改，否则客户端不应该重复提交这个请求。
2、请求参数有误。

401 Unauthorized

当前请求需要用户验证。该响应必须包含一个适用于被请求资源的 WWW-Authenticate 信息头用以询问用户信息。客户端可以重复提交一个包含恰当的 Authorization 头信息的请求。如果当前请求已经包含了 Authorization 证书，那么401响应代表着服务器验证已经拒绝了那些证书。如果401响应包含了与前一个响应相同的身份验证询问，且浏览器已经至少尝试了一次验证，那么浏览器应当向用户展示响应中包含的实体信息，因为这个实体信息中可能包含了相关诊断信息。

402 Payment Required

此响应码保留以便将来使用，创造此响应码的最初目的是用于数字支付系统，然而现在并未使用。

403 Forbidden

服务器已经理解请求，但是拒绝执行它。与 401 响应不同的是，身份验证并不能提供任何帮助，而且这个请求也不应该被重复提交。如果这不是一个 HEAD 请求，而且服务器希望能够讲清楚为何请求不能被执行，那么就应该在实体内描述拒绝的原因。当然服务器也可以返回一个 404 响应，假如它不希望让客户端获得任何信息。

404 Not Found

请求失败，请求所希望得到的资源未被在服务器上发现。没有信息能够告诉用户这个状况到底是暂时的还是永久的。假如服务器知道情况的话，应当使用410状态码来告知旧资源因为某些内部的配置机制问题，已经永久的不可用，而且没有任何可以跳转的地址。404这个状态码被广泛应用于当服务器不想揭示到底为何请求被拒绝或者没有其他适合的响应可用的情况下。

405 Method Not Allowed

请求行中指定的请求方法不能被用于请求相应的资源。该响应必须返回一个Allow 头信息用以表示出当前资源能够接受的请求方法的列表。 鉴于 PUT，DELETE 方法会对服务器上的资源进行写操作，因而绝大部分的网页服务器都不支持或者在默认配置下不允许上述请求方法，对于此类请求均会返回405错误。

406 Not Acceptable

请求的资源的内容特性无法满足请求头中的条件，因而无法生成响应实体。

407 Proxy Authentication Required

与401响应类似，只不过客户端必须在代理服务器上进行身份验证。代理服务器必须返回一个 Proxy-Authenticate 用以进行身份询问。客户端可以返回一个 Proxy-Authorization 信息头用以验证。

408 Request Timeout

请求超时。客户端没有在服务器预备等待的时间内完成一个请求的发送。客户端可以随时再次提交这一请求而无需进行任何更改。

409 Conflict

由于和被请求的资源的当前状态之间存在冲突，请求无法完成。这个代码只允许用在这样的情况下才能被使用：用户被认为能够解决冲突，并且会重新提交新的请求。该响应应当包含足够的信息以便用户发现冲突的源头。

410 Gone

被请求的资源在服务器上已经不再可用，而且没有任何已知的转发地址。这样的状况应当被认为是永久性的。如果可能，拥有链接编辑功能的客户端应当在获得用户许可后删除所有指向这个地址的引用。如果服务器不知道或者无法确定这个状况是否是永久的，那么就应该使用 404 状态码。除非额外说明，否则这个响应是可缓存的。

411 Length Required

服务器拒绝在没有定义 Content-Length 头的情况下接受请求。在添加了表明请求消息体长度的有效 Content-Length 头之后，客户端可以再次提交该请求。

412 Precondition Failed

服务器在验证在请求的头字段中给出先决条件时，没能满足其中的一个或多个。这个状态码允许客户端在获取资源时在请求的元信息（请求头字段数据）中设置先决条件，以此避免该请求方法被应用到其希望的内容以外的资源上。

413 Payload Too Large

服务器拒绝处理当前请求，因为该请求提交的实体数据大小超过了服务器愿意或者能够处理的范围。此种情况下，服务器可以关闭连接以免客户端继续发送此请求。如果这个状况是临时的，服务器应当返回一个 Retry-After 的响应头，以告知客户端可以在多少时间以后重新尝试。

414 URI Too Long

请求的URI 长度超过了服务器能够解释的长度，因此服务器拒绝对该请求提供服务。这比较少见，通常的情况包括：本应使用POST方法的表单提交变成了GET方法，导致查询字符串（Query String）过长。

415 Unsupported Media Type

对于当前请求的方法和所请求的资源，请求中提交的实体并不是服务器中所支持的格式，因此请求被拒绝。

416 Range Not Satisfiable

如果请求中包含了 Range 请求头，并且 Range 中指定的任何数据范围都与当前资源的可用范围不重合，同时请求中又没有定义 If-Range 请求头，那么服务器就应当返回416状态码。

417 Expectation Failed

此响应代码意味着服务器无法满足 Expect 请求标头字段指示的期望值。

418 I'm a teapot

服务器拒绝尝试用 “茶壶冲泡咖啡”。客户端错误响应代码表示服务器拒绝冲泡咖啡，因为它是一个茶壶。 这个错误是超文本咖啡壶控制协议的参考。注意，这是一个1998年的愚人节笑话，请不要在正式报文中使用它，这会导致客户端陷入困惑并想冲泡一杯咖啡。

421 Misdirected Request

该请求针对的是无法产生响应的服务器。 这可以由服务器发送，该服务器未配置为针对包含在请求 URI 中的方案和权限的组合产生响应。

422 Unprocessable Entity (WebDAV)

请求格式良好，但由于语义错误而无法遵循。

423 Locked (WebDAV)

正在访问的资源被锁定。

424 Failed Dependency (WebDAV)

由于先前的请求失败，所以此次请求失败。

425 Too Early

服务器不愿意冒着风险去处理可能重播的请求。

426 Upgrade Required

服务器拒绝使用当前协议执行请求，但可能在客户机升级到其他协议后愿意这样做。 服务器在 426 响应中发送 Upgrade (en-US) 头以指示所需的协议。

428 Precondition Required

原始服务器要求该请求是有条件的。 旨在防止“丢失更新”问题，即客户端获取资源状态，修改该状态并将其返回服务器，同时第三方修改服务器上的状态，从而导致冲突。

429 Too Many Requests

用户在给定的时间内发送了太多请求（“限制请求速率”）。

431 Request Header Fields Too Large

服务器不愿意处理请求，因为它的 请求头字段太大（ Request Header Fields Too Large）。 请求可以在减小请求头字段的大小后重新提交。

451 Unavailable For Legal Reasons

用户请求非法资源，例如：由政府审查的网页。
服务端响应

500 Internal Server Error

服务器遇到了不知道如何处理的情况。

501 Not Implemented

此请求方法不被服务器支持且无法被处理。只有GET和HEAD是要求服务器支持的，它们必定不会返回此错误代码。

502 Bad Gateway

此错误响应表明服务器作为网关需要得到一个处理这个请求的响应，但是得到一个错误的响应。

503 Service Unavailable

服务器没有准备好处理请求。 常见原因是服务器因维护或重载而停机。 请注意，与此响应一起，应发送解释问题的用户友好页面。 这个响应应该用于临时条件和 Retry-After：如果可能的话，HTTP头应该包含恢复服务之前的估计时间。 网站管理员还必须注意与此响应一起发送的与缓存相关的标头，因为这些临时条件响应通常不应被缓存。

504 Gateway Timeout

当服务器作为网关，不能及时得到响应时返回此错误代码。

505 HTTP Version Not Supported

服务器不支持请求中所使用的HTTP协议版本。

506 Variant Also Negotiates

服务器有一个内部配置错误：对请求的透明内容协商导致循环引用。

507 Insufficient Storage

服务器有内部配置错误：所选的变体资源被配置为参与透明内容协商本身，因此不是协商过程中的适当端点。

508 Loop Detected (WebDAV)

服务器在处理请求时检测到无限循环。

510 Not Extended

客户端需要对请求进一步扩展，服务器才能实现它。服务器会回复客户端发出扩展请求所需的所有信息。

511 Network Authentication Required

511 状态码指示客户端需要进行身份验证才能获得网络访问权限。