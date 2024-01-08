---
title: Web 前端开发复习
date: 2023-02-22
tags:
  - frontend
categories:
  - study-notes
---

<!--more-->

# Web 前端开发复习

> 声明：本文部分内容来自 ChatGPT，请谨慎阅读。

## Introduction

### 前端技术栈

![image-20230219170053103](https://cdn.hcplantern.cn/img/2023/02/19/20230219-170054.png-default)

### 前端学习路线

[Frontend Developer Roadmap](https://roadmap.sh/frontend)

### 浏览器

**浏览器内核：**

Gecko：早期被 Netscape 和 Mozilla Firefox 浏览器浏览器使用；

Trident：微软开发，被 IE4~IE11 浏览器使用，但是 Edge 浏览器已经转向 Blink； 

Webkit：苹果基于 KHTML 开发、开源的，用于 Safari，Google Chrome 之前也在使用； 

Blink：是 Webkit 的一个分支，Google 开发，目前应用于 Google Chrome、Edge、Opera 等；

**布局引擎：**



**JS 引擎：**

- SpiderMonkey：第一款 JavaScript 引擎，由 Brendan Eich 开发（JavaScript 作者）；
- Chakra：用于 IE 浏览器，由微软开发；
- JavaScriptCore：Webkit 中内置的 JavaScript 引擎，由苹果公司开发；
- V8：目前最为强大和流行的 JavaScript 引擎，由 Google 开发；

## 名词解释

### IP

Internet Protocol，计算机间进行数据交换最简单的协议。也就是计算机网络互联进行通信而设计的协议，在因特网中，它能使连接到网上所有计算机网络实现相互通信的一套规则，规定了计算机在因特网上进行通信时应当遵守的规则。

### URI

Uniform Resource Identifier，统一资源标识符。表示的是 Internet 上每一种可用的资源，如 HTML 文档、图像、视频片段、程序等都由一个 URI 进行标识的。

### URL

Uniform Resource Locator，统一资源定位符。在 WWW 上，每一信息资源都有统一的且在网上唯一的地址，该地址就叫 URL。它是 WWW 的统一资源定位标志，就是指网络地址。

### URN

Uniform Resource Name，统一资源名。每一个资源独立的名称。

### DNS

Domain Name System，域名系统。互联网上作为域名和 IP 地址互相映射的一个分布式数据库。

### HTTP

Hypertext Transfer Protocol，超文本传输协议。通常运行在 TCP 之上。一种详细规定了浏览器和万维网服务器之间互相通信的规则，通过因特网传送万维网文档的数据传送协议。

### RFC

Request For Comments，是一系列以编号排定的文件。文件**收集了有关互联网相关信息，以及 UNIX 和互联网社区的软件文件**。目前 RFC 文件是由 Internet Society（ISOC）赞助发行。基本的互联网通信协议都有在 RFC 文件内详细说明。

### HTML

Hypertext Markup Language，超文本标记语言。是用于描述网页文档的一种标记语言，浏览器根据 html 创建 web 页面与 web 应用。

### TLS & SSL

TLS（Transport Layer Security）和 SSL（Secure Sockets Layer）都是用于保障网络通信安全的协议。SSL 是 TLS 的前身，TLS 是 SSL 的标准化版本。TLS/SSL 协议在应用层和传输层之间建立一个加密的通道，确保了通信数据的机密性、完整性和可靠性。常见的应用包括 HTTPS 协议、SMTPS 协议、POP3S 协议和 IMAPS 协议等。

### SGML

Standard Generalized Markup Language，标准通用标记语言。是一种非常弹性的标记语言，是一种定义电子文档结构和描述其内容的国际标准语言。

### XHTML

eXtensible HyperText Markup Language，可扩展超文本标识语言。HTML 是一种基于标准通用标记语言（SGML）的应用，是一种非常灵活的置标语言，而 XHTML 则基于可扩展标记语言（XML），XML 是 SGML 的一个子集。

### Markup

标记，将代码嵌入到文本中，代码叫做 tag。代码用来表示结构化文本并包含运行指令

### Markup language

标记语言，是一种将文本以及文本相关的其他信息结合起来，展现出关于文档结构和数据处理细节的电脑文字编码。可以使用其他工具进行特定的渲染。

### Logical Markup

逻辑标记。用来形容一部分文档的，与渲染无关。例如：\<strong>very\</strong>表示在文档中 very 很重要

### Markdown 

是⼀种轻量级标记语⾔，它允许⼈们使⽤易读易写的纯⽂本格式编写⽂档。编写的⽂档可以导出 HTML 、Word、图像、PDF、Epub 等多种格式的⽂档。编写的⽂档后缀为 .md, .markdown。

### CSS

Cascading Style Sheets，层叠样式表。用来表现 HTML（标准通用标记语言的一个应用）或 XML（标准通用标记语言的一个子集）等文件样式的计算机语言。CSS 不仅可以静态地修饰网页，还可以配合各种脚本语言动态地对网页各元素进行格式化。

### W3C

World Wide Web Consortium，万维网联盟。创建于 1994 年，是 Web 技术领域最具权威和影响力的国际中立性技术标准机构。

### DOM

Document Object Model，文档对象模型。它是一种与平台和语言无关的应用程序接口 (API), 它可以动态地访问程序和脚本, 更新其内容、结构和 www文档的风格 (目前, HTML 和 XML 文档是通过说明部分定义的)。文档可以进一步被处理，处理的结果可以加入到当前的页面。

### BOM

Browser Object Model，浏览器对象模型。是用于描述这种对象与对象之间层次关系的模型，浏览器对象模型提供了独立于内容的、可以与浏览器窗口进行互动的对象结构。BOM 由多个对象组成，其中代表浏览器窗口的 Window 对象是 BOM 的顶层对象，其他对象都是该对象的子对象。

### cookies

类型为“小型文本文件”，是某些网站为了辨别用户身份，进行 Session 跟踪而储存在用户本地终端上的数据（通常经过加密），由用户客户端计算机暂时或永久保存的信息。

### SEO

Search Engine Optimization，搜索引擎优化。利用搜索引擎的规则提高网站在有关搜索引擎内的自然排名。目的是让其在行业内占据领先地位，获得品牌收益。很大程度上是网站经营者的一种商业行为，将自己或自己公司的排名前移。

### WPO

web performance optimization，web 性能优化。⽹站越快,⽤户的黏性越⾼;（参与度）。⽹站越快,⽤户忠诚度更⾼; （留存率）。⽹站越快,⽤户转化率越⾼。

### CDN & CDS

Content Delivery Network (Service)，即内容分发网络（服务）。CDN 是构建在现有网络基础之上的智能虚拟网络，依靠部署在各地的边缘服务器，通过中心平台的负载均衡、内容分发、调度等功能模块，使用户就近获取所需内容，降低网络拥塞，提高用户访问响应速度和命中率。CDN 的关键技术主要有内容存储和分发技术。

### SPA

Single Page application，单页应用，是一种网络应用程序或网站的模型。它通**过动态重写当前页面来与用户交互，而非传统的从服务器重新加载整个新页面。**这种方法避免了页面之间切换打断用户体验，使应用程序更像一个桌面应用程序。

### robot. txt

robots. txt 是一种存放于网站根目录下的文本文件，它通常规定了搜索引擎抓取工具（爬虫）可以访问网站上的哪些网址。这个协议不是一个规范，而只是约定俗成的，有些搜索引擎会遵守这一规范，有些则不然。

### RIA

Rich Internet Applications 富 Internet 应用程序，通过使用前端技术（如 JavaScript、Ajax 等）和丰富的交互式元素（如动态图表、拖放、弹出窗口等），实现在发送请求后仅从服务器获取必要的数据，而不需要重新加载整个页面。这样可以提高应用程序的性能和响应速度，并提供更流畅和自然的用户体验。

### AJAX

Asynchronous JavaScript And XML，是一种用于异步处理数据的技术，能够在不刷新整个页面的情况下更新部分页面内容，提高 Web 应用程序的响应速度和用户体验。

### SOP

Same Origin Policy, 同源策略，相同协议、域名、端口的请求才被允许，限制非同源资源的获取。

### CORS

Cross-Origin Resource Sharing，一种基于 HTTP 头的机制，它允许服务器指定哪些源可以访问资源，同时允许浏览器提交跨域请求。服务器如果设置了 CORS 相关配置，在返回浏览器的请求头会加上 `Access-Control-Allow-Origin`，浏览器看到这个字段的值与当前的源匹配，就会解锁跨域限制。

### SSO

Single Sign On，单点登陆，在同域下的客户端/服务端认证系统中，通过客户端携带凭证，可以维持⼀段时间内的登录状态。

## Internet and WWW

Web 1.0 2.0 3.0 比较 

- 1.0 **门户时代**。Web 1.0 中只有少数几个内容创建者，大多数用户都是内容的消费者。
- 2.0 **搜索社交时代**，网站开始支持动态页面、异步加载、多媒体交互等特性，用户可以积极参与网站内容的创作和共享，形成了众多的社交网络、博客、维基百科、视频网站等。
- 3.0 **大互联时代**，也称为“语义 Web”，指的是未来互联网的发展趋势。Web 3.0 采用语义技术和人工智能技术，通过为网页赋予“含义”，实现数据的智能化处理和自动化交互。Web 3.0 的应用场景包括语音助手、智能家居、智能交通、智能医疗等领域。

Web 3.0 的特点：

1. 智能化处理：采用**语义技术和人工智能技术**，为网页赋予“含义”，实现数据的智能化处理和自动化交互。
2. 去中心化：Web 3.0 也是一个去中心化的互联网，不再依赖中心化的服务器和数据中心，而是采用区块链和去中心化技术，实现分布式存储和计算。
3. 开放互联：Web 3.0 建立在开放互联的原则上，促进不同系统和应用之间的数据共享和交互。这样能够提高数据的可靠性和透明度，促进不同应用的集成和创新。
4. 多媒体融合：Web 3.0 也是一个多媒体融合的互联网，能够将不同类型的媒体内容，如文本、图像、音频、视频等有机地融合在一起，从而提供更加丰富和多样化的用户体验。
5. 社交互动：Web 3.0 也是一个社交互动的互联网，能够促进用户之间的交流和合作，增强用户的参与和共享。这样能够提高网站的活跃度和用户满意度，促进网站的发展和壮大。

## HTML

### 基本标签

- `<title>`
- `<meta>`
  - HTML 文档的元数据，网页描述、关键词、文档作者、最后修改日期等
  - 只能出现在 `<head>` 里
- `<h1> - <h6>`
- `<p>`
- `<br>`
- `<a>`：超链接，属性 `href`
- `<img>`
- `<em>, <strong>`：强调、重要文本
- Table: `<table>, <tr>, <td>, <th>, <caption>`
- `<blockquote>` 块元素
- `q` 行内引用
- `<form>` 表单

### 块级元素和行内元素

**块级元素**占据其⽗元素（容器）的**整个⽔平空间**，垂直空间等于其内容⾼度，因此创建了⼀个“块”

- 例如: paragraphs, lists, table cells
- 通常浏览器会在块级元素前后另起⼀个新⾏

**⾏内元素**只占据它对应标签的边框所包含的空间。 

- 例如: bold text, code fragments, images
- 浏览器允许许多⾏内元素出现在同⼀⾏上 
- 必须嵌套在块级元素中

### HTML5

HTML5 是下⼀代 HTML 标准。

#### 优势

- 跨浏览器兼容性
- ⼤道⾄简：简化 DOCTYPE、标签、字符集
- 语义化：根据结构化的内容选择合适的标签
- 可⽤性和⽤户体验的改进 
  - HTML5 为 Web 开发⼈员提供了多项技术改进和改进的功能。使⽤ HTML5 代码，Web 开发⼈员可以轻松设计更好的应⽤程序和动态⽹站，从⽽带来更好的⽤户体验。 
  - ⽆障碍是⼀种让尽可能多的⽤户可以使⽤你的⽹站的做法。传统上我们认为这只与残疾⼈⼠有关，但提升⽹站的⽆障碍也可以让其他⽤户群体受益。⽐如使⽤移动设备的⼈群，那些使⽤低速⽹络连接的⼈群。
- ⽆插件范式：过去，很多功能只能通过插件或者复杂的 hack (本地绘图 API、本地 socket 等) 来实现，但是在 HTML5 中提供了对这些功能的原⽣⽀持，视频音频图像等
- 安全：为 iframe 元素增加 sandbox 属性；跨源资源共享 (CORS) 使 Web 服务器允许其他域名的⽹⻚访问⾃⼰的资源
- ⼤量⽤于移动应⽤和游戏 
- 在⽹站抓取和索引⽅⾯，HTML5 对 SEO 友好

#### 应用

- 本地数据存储
- 访问本地⽂件
- 本地 SQL 数据缓存引⽤ 
- Javascript ⼯作者 
- XHTML HttpRequest 2

#### 新元素

![image-20230220154410026](https://cdn.hcplantern.cn/img/2023/02/20/20230220-154411.png-default)

#### Canvas SVG

- SVG 是⼀种⽤XML 描述 2D 图形的语⾔
- Canvas 绘制 2D 图像，使⽤JavaScript 
- SVG 是基于 XML 的，这意味着 SVG DOM 中的每个元素都是可⽤的。可以为元素附加 JavaScript 事件处理程序 
- 在 SVG 中，每个绘制的形状都作为⼀个对象。如果 SVG 对象的属性发⽣了更改，浏览器可以⾃动重新渲染形状 
- Canvas 是⼀个像素⼀个像素渲染的。在 canvas 中，⼀旦绘制出图形，它就会被浏览器遗忘。如果它的位置需要改变，整个场景需要重新绘制，包括可能被图形覆盖的任何对象

![image-20230220154553083](https://cdn.hcplantern.cn/img/2023/02/20/20230220-154554.png-default)

#### 其他新功能

HTML5 应用程序缓存：使用 cache manifest 创建 web 应用的离线版本

Web SQL 数据库；

Web Workers：运行在后台的 JS，独立与其他脚本

WebSocket：服务端现在可以主动向客户端发送报文了

### Web 语义化

Web 语义化是指使用**恰当语义的 html 标签、class 类名等内容，让页面具有良好的结构与含义**，从而让人和机器都能快速理解网页内容。

语义化的 web 页面一方面可以让机器在更少的人类干预情况下收集并研究网页的信息，从而可以读懂网页的内容，然后将收集汇总的信息进行分析，结果为人类所用；另一方面它可以让开发人员读懂结构和用户以及屏幕阅读器（如果访客有视障）能够读懂内容。简单来说就是**利于 SEO，便于阅读维护理解**。

HTML 语义化是反对大篇幅使用无语义化的 div+span+class，而**鼓励使用 HTML 定义好的语义化标签**。

### HTML5 Web 存储

**LocalStorage、LocalSession、cookies 的区别？**

Cookies、Local Storage 和 Session Storage 都是 Web 前端常用的数据存储方式，它们的作用都是为了在客户端（浏览器）存储数据，方便在之后的访问中使用。

它们的联系和区别如下：

1. 存储位置：

- Cookies：以文本文件的形式存储在客户端的浏览器中，每次请求都会携带 Cookie 数据到服务端。
- Local Storage：存储在客户端的浏览器中，不会随着 HTTP 请求发送到服务器。
- Session Storage：存储在客户端的浏览器中，仅在当前浏览器窗口或标签页中有效，关闭窗口或标签页后数据会被删除。

2. 存储大小限制：

- Cookies：每个 Cookie 的大小一般不能超过 4KB，每个域名下的 Cookie 总大小一般不能超过 20 个。
- Local Storage 和 Session Storage：每个域名下的存储大小限制一般在 5MB 左右。

3. 数据的读写操作：

- Cookies：由服务器设置，可以通过 JavaScript 读取和修改。
- Local Storage 和 Session Storage：可以通过 JavaScript 直接读取和修改。

4. 生命周期：

- Cookies：可以设置过期时间，如果没有设置过期时间则会在浏览器关闭后自动删除。
- Local Storage 和 Session Storage：除非被主动清除，否则数据会一直保存在客户端中。

5. 使用场景：

- Cookies：适用于**跨页面、跨域名、跨子域名的数据传输**，比如用户登录信息、购物车等。
- Local Storage：适用于在**同一个页面中存储较大量的数据**，比如表单数据、网站配置等。
- Session Storage：适用于**同一页面中的临时数据存储**，比如表单中的数据等。

总结：Cookies 适用于需要持久化的数据，Local Storage 适用于需要本地存储的数据，Session Storage 适用于需要在同一页面中临时存储的数据。

## CSS

### 基本概念

CSS 是层叠样式表（Cascading Style Sheets）的缩写，是一种用于描述网页样式的语言。CSS 的基本概念包括：

1. 选择器（Selectors）：指定要应用样式的 HTML 元素，可以是标签名、类名、ID 或者其它属性等。
2. 属性（Properties）：定义要应用的样式效果，如颜色、字体、背景等。
3. 值（Values）：指定属性要设置的具体值，如颜色名称、大小、类型等。
4. 盒模型（Box Model）：描述 HTML 元素在页面中占据的空间，由外边距（margin）、边框（border）、内边距（padding）、内容区域（content）组成。
5. 文本排版（Typography）：包括字体、大小、颜色、行高等，用于控制文本在页面中的排版效果。
6. 布局（Layout）：用于控制 HTML 元素在页面中的位置、大小等布局属性。
7. 伪类（Pseudo-class）：用于指定元素在特定状态下的样式效果，如鼠标悬停时、被访问时等。
8. 继承（Inheritance）：CSS 样式属性可以从父元素继承，避免了重复定义样式的问题。
9. 层叠（Cascading）：当多个选择器定义相同的样式属性时，CSS 根据选择器的优先级和权重来确定最终的样式效果。

### 选择器

CSS 选择器用于选择要应用样式的 HTML 元素，可以根据元素的标签名、类名、ID、属性等进行选择。以下是常用的 CSS 选择器介绍：

1. 标签选择器（Type Selector）：通过 HTML 元素的标签名进行选择，如 `p` 选择所有 `<p>` 标签。
2. 类选择器（Class Selector）：通过 HTML 元素的 class 属性进行选择，以 `.` 开头，如 `.red` 选择所有 class 属性值为 `red` 的元素。
3. ID 选择器（ID Selector）：通过 HTML 元素的 id 属性进行选择，以 `#` 开头，如 `#header` 选择 id 属性值为 `header` 的元素。
4. 后代选择器（Descendant Selector）：通过元素的嵌套层次关系进行选择，如 `div p` 选择所有在 `<div>` 元素内的 `<p>` 元素。
5. 通用选择器（Universal Selector）：选择所有元素，以 `*` 表示，如 `*` 选择所有元素。
6. 伪类选择器（Pseudo-class Selector）：用于选择元素的特定状态，如 `:hover` 选择鼠标悬停在元素上的状态，`:visited` 选择已访问链接的状态。
7. 伪元素选择器（Pseudo-element Selector）：用于选择元素的某些特定部分，如 `::before` 选择元素前面插入的内容，`::after` 选择元素后面插入的内容。

除了常用的选择器之外，CSS 还有一些高级选择器可以根据不同的需求选择特定的 HTML 元素，以下是一些常用的高级选择器：

1. 子选择器（Child Selector）：用于选择某个元素的直接子元素，如 `ul > li` 选择 `<ul>` 元素下的所有直接子元素 `<li>`。
2. 相邻兄弟选择器（Adjacent Sibling Selector）：选择紧接在另一个元素后的兄弟元素，如 `h1 + p` 选择所有在 `<h1>` 元素后的第一个 `<p>` 元素。
3. 通用兄弟选择器（General Sibling Selector）：选择与某个元素相邻的所有兄弟元素，如 `h1 ~ p` 选择所有在 `<h1>` 元素后的所有 `<p>` 元素。
4. 属性值选择器（Attribute Value Selector）：根据属性值来选择元素，如 `[href^="https://"]` 选择所有 `href` 属性值以 `https://` 开头的元素。
5. 否定选择器（Negation Selector）：选择除了特定元素以外的所有元素，如 `:not(p)` 选择除了 `<p>` 元素以外的所有元素。
6. : first-child 选择器：选择父元素的第一个子元素，如 `p:first-child` 选择每个父元素的第一个子元素 `<p>`。
7. : last-child 选择器：选择父元素的最后一个子元素，如 `p:last-child` 选择每个父元素的最后一个子元素 `<p>`。

### 层叠排序

CSS 中的层叠排序（Cascading Order）指的是浏览器**如何确定多个样式规则中哪一个规则应该被应用到同一个 HTML 元素上**。层叠排序的顺序如下：

1. 优先级（Specificity）：根据选择器的特殊性（特定程度）来决定优先级，特殊性越高的样式规则优先级越高。特殊性的计算方法可以参考 CSS 选择器中特殊性计算的相关规则。
2. 重要性（Important）：通过在样式规则中添加 `!important` 标记来指定该规则的优先级最高。
3. 源代码顺序（Source Order）：当以上两个条件相同时，根据样式规则在源代码中的顺序来决定优先级，后面的样式规则优先级更高。

层叠排序的概念和规则可以帮助开发者更好地控制样式规则的优先级，确保应用样式的正确性和可维护性。同时，由于优先级和重要性等因素的影响，应该尽可能避免使用 `!important` 标记，以免影响其他样式规则的优先级。

### CSS 优先级

CSS 优先级可以通过以下三个因素来确定：

1. 样式来源：浏览器会对不同来源的样式表进行不同的优先级排序。样式来源优先级由高到低排列：

   - 通过用户浏览器设置的样式表。
   - inline CSS
   - 内部 CSS `<style>` tag
   - 外部 `<link>` & `@import`
   - 浏览器默认样式表。

2. 选择器的特殊性：CSS 选择器的特殊性决定了选择器的优先级。通常，选择器的特殊性越高，优先级越高。在计算特殊性时，CSS 将选择器分解为以下四个等级：

   1. 行内样式：对应 1000，即具有最高特殊性。
   2. ID 选择器：对应 100。
   3. 类选择器、属性选择器、伪类选择器：对应 10。
   4. 元素选择器、伪元素选择器：对应 1。

   特殊性计算方法如下：

   1. 对于选择器中包含的 ID 选择器，将其个数乘以 100；
   2. 对于选择器中包含的类选择器、属性选择器、伪类选择器，将其个数乘以 10；
   3. 对于选择器中包含的元素选择器、伪元素选择器，将其个数乘以 1；
   4. 对于行内样式，直接将特殊性值设置为 1000。

   将每个选择器的特殊性值相加即可得到整个选择器的特殊性值。例如，下面的选择器：

   ```css
   div#myId.myClass {
     color: red;
   }
   ```

   其中包含 1 个 ID 选择器、1 个类选择器和 1 个元素选择器，因此它的特殊性值为 100 + 10 + 1 = 111。

3. 样式的位置：在样式表中，后面的样式会覆盖前面的样式。

### CSS3

CSS3 是 CSS 的第三个主要版本，于 1999 年开始开发，目前已经成为 Web 前端开发的重要技术之一。CSS3 在之前版本的基础上引入了许多新的特性和模块，增加了对页面布局、样式、动画、交互等方面的支持，使得开发者能够更加轻松、高效地实现各种效果和交互。

以下是 CSS3 的一些主要特性：

1. 新的布局方式：CSS3 引入了弹性盒子（Flexbox）和网格布局（Grid）
2. 新的样式属性：CSS3 引入了许多新的样式属性，如 border-radius、box-shadow、text-shadow 等
3. 新的选择器：CSS3 引入了许多新的选择器，如属性选择器、伪类选择器、伪元素选择器等
4. 动画和过渡：CSS3 提供了动画和过渡的支持，使得开发者能够更加简单地实现页面元素的动态效果。
5. 媒体查询：CSS3 引入了媒体查询，使得开发者能够根据不同设备的特性为不同设备提供不同的样式。
6. 字体和排版：CSS3 提供了更加丰富的字体和排版支持，如字体加载、文本溢出、多列布局等

除了上述特性之外，CSS3 还引入了许多其他新的特性和模块，如变形（Transform）、多背景（Multiple Backgrounds）、滤镜（Filter）、网页字体（Web Fonts）等。

### CSS Sprites

CSS Sprites 是一种优化网页加载速度的技术。它是将**多个小图标或背景图像合并为一个大图像**，通过 CSS 属性控制显示的位置来实现页面中各个图像的显示。

具体来说，将多个小图标或背景图像合并为一个大图像，可以减少 HTTP 请求次数，从而减少页面加载时间。而使用 CSS Sprites，可以通过 background-position 属性来控制显示的位置，从而显示所需的小图标或背景图像。

使用 CSS Sprites 可以**有效减少网页的加载时间**，提升网页的性能和用户体验。但是，它**需要一定的技术和设计能力**，因为需要将多个小图标或背景图像合并为一个大图像，并在 CSS 中设置正确的坐标。

### CSS 布局

CSS 提供了多种布局方式，可以根据不同的需求选择合适的布局方式来实现页面的排版和布局。下面介绍几种常用的布局方式：

1. 正常布局流（normal flow：是指在不对页面进行任何布局控制时，浏览器默认的 HTML 布局方式。
2. 浮动布局（Float Layout）：通过设置页面元素的浮动属性，可以让元素脱离文档流，并按照一定的规则排列。浮动布局常用于实现图片排列、文字环绕等效果。
3. 弹性盒子布局（Flexbox Layout）：弹性盒子布局是一种现代化的布局方式，通过设置容器元素的 flex 属性和项目元素的 flex 属性，可以实现各种复杂的页面布局。
4. 网格布局（Grid Layout）：网格布局是一种二维布局方式，通过设置容器元素的 grid 属性和项目元素的 grid 属性，可以实现各种复杂的页面布局。
5. 定位布局（Position Layout）：通过设置页面元素的定位属性（如 position、top、left、right、bottom 等），可以实现绝对定位、相对定位等布局效果。

### 盒模型

CSS 盒模型是用来描述 HTML 元素在页面中所占空间的一个概念。每个 HTML 元素都被看作一个矩形盒子，由内容区、内边距（padding）、边框（border）和外边距（margin）四个部分组成。这四个部分都可以通过 CSS 来控制它们的大小和样式。

![image-20230220222618119](https://cdn.hcplantern.cn/img/2023/02/20/20230220-222619.png-default)

### BFC

BFC 是 Block Formatting Context（块级格式化上下文）的缩写，是 CSS 布局中的一个重要概念。它是一个**独立的渲染区域，内部的元素按照一定规则进行布局，同时也不会影响到外部元素的布局**。

BFC 最常见的应用是解决浮动元素造成的布局问题。当一个元素的 float 属性不为 none，且该元素有相邻的兄弟元素时，浮动元素会脱离文档流，可能会对其它元素造成影响，导致布局混乱。这时可以将这个元素所在的区域生成 BFC，这个区域内部的元素将按照 BFC 的规则进行布局，而不受浮动元素的影响。

BFC 的规则如下：

1. BFC 内部的元素在垂直方向上一个接一个地排列，每个元素占据一行。
2. BFC 内部的元素之间不会重叠，而是从父元素的顶部开始依次往下布局。
3. BFC 内部的元素的上下外边距（margin）不会发生重叠，而是取最大值。
4. BFC 内部的元素可以通过设置 overflow 属性来清除浮动。

生成 BFC 的方式有以下几种：

1. 设置元素的 display 属性为 inline-block、table-cell、table-caption 等值。
2. 设置元素的 position 属性为 absolute 或 fixed。
3. 设置元素的 float 属性为 left 或 right。
4. 设置元素的 overflow 属性为 auto、scroll 或 hidden。

### 响应式 Web 设计

响应式 Web 设计（Responsive Web Design，RWD）是一种可以适应不同屏幕大小和分辨率的 Web 设计技术。其基本原理是通过 **CSS3 Media Queries 和流式布局**来实现，使得 Web 页面能够在不同的设备上显示出最佳的效果，提供更好的用户体验。

传统的 Web 设计通常是为桌面电脑设计的，当页面在移动设备上打开时，可能会出现排版混乱、图片过大等问题。响应式 Web 设计通过使用**流式布局、媒体查询**等技术，可以根据设备的屏幕尺寸和分辨率来**动态调整页面的布局、字体大小、图片大小**等，从而在不同的设备上呈现出最佳的效果。

响应式 Web 设计的优点包括：

1. 提高了用户体验，可以让用户在不同的设备上都能够方便地浏览和操作网站。
2. 减少了开发和维护的成本，因为只需要设计和维护一个网站即可适配多种设备。
3. 有助于提高网站的可访问性和 SEO。

缺点：工作量大、效率低下、限制应用的复杂性

### 移动优先

移动优先是一种 Web 设计和开发的思想，其基本原则是**优先考虑移动设备的用户体验和需求**，而不是将移动设备作为桌面设备的附属品。移动优先的设计可以让 Web 页面在移动设备上更加流畅、易用和快速，同时也可以提高页面的可访问性和可用性。

移动优先的设计可以包括以下几个方面：

1. **简化页面布局和内容**：移动设备屏幕较小，需要简化页面布局和内容，减少不必要的元素和功能，优先显示最重要的内容和功能，如果空间允许，再逐步加⼊次要内容和功能，从而提高页面的加载速度和响应性能。
2. 提供**清晰的导航和操作方式**：移动设备上的操作方式和导航方式与桌面设备有所不同，需要提供清晰的导航和操作方式，以便用户可以方便地浏览和使用网站。
3. 采用**响应式 Web 设计**：使用响应式 Web 设计可以让页面在不同设备上都能够呈现出最佳的效果，从而提高页面的可用性和用户体验。
4. 优化**页面的加载速度**：移动设备上的网络环境和速度有限，需要优化页面的加载速度，减少不必要的资源请求和压缩页面大小等方式来提高页面性能。

移动优先的最佳实践：

- 使⽤有效的，⼴泛⽀持的**HTML5 和 CSS3**来构建你的站点。
- 利⽤**HTML5 新语义元素和 ARIA**为内容添加意义、可访问性和搜索可⻅性。
- 在布局和排版中使⽤⽐例度量，如**百分⽐和 em**。
- 根据⽤户的需求、观看设备的可能范围以及特定内容的性质，计算响应断点并使⽤**媒体查询**。
- 使⽤移动优先的⽅法，建⽴最⼩和最基本的体验。
- 根据屏幕空间、带宽和浏览器功能，逐步提升移动优先的体验。
- 在研发周期中**优先考虑浏览器因素**，尽量避免使⽤复杂的、⾯向桌⾯的 Photoshop 排版和静态图形设计。花哨的静态设计模型是⼀种过时的思维⽅式，与现代 web 实现完全脱节。

### 优雅降级和渐进渐强

**一个是从全功能降级到低功能；一个是从基本功能开始逐步增加新功能**

优雅降级是指为了让较旧的浏览器和设备能够正确显示网站，而在功能上做出一些妥协的过程。在这种情况下，你**首先设计网站的完整功能，然后根据用户设备和浏览器的功能逐步退化**。这种方法通常会带来更多的工作，但可以确保在旧设备上仍能提供有用的信息和功能。

渐进增强（Progressive Enhancement）是一种 Web 设计和开发的思想，它强调在网站的设计和开发中，应该**从最基础的功能开始，逐步增强功能和体验**。渐进增强的设计可以使网站具有更好的可访问性、可用性和可维护性。

渐进增强的设计思想可以分为以下几个阶段：

1. 基础功能：从网站的基本功能开始设计，例如网页的结构、内容和样式等。
2. 可用性增强：通过添加一些额外的功能和交互元素，提高网站的可用性和用户体验，例如响应式布局、菜单导航和表单验证等。
3. 视觉增强：通过添加一些视觉效果和动画效果，提高网站的用户体验和吸引力，例如背景图片、颜色渐变和过渡效果等。
4. 进阶增强：通过添加更复杂的功能和特性，例如 Ajax、WebGL 和 WebAssembly 等，进一步提升网站的性能和用户体验。

## Javascript

### 基本概念

### DOM BOM

DOM 是指文档对象模型，是浏览器将 HTML 或 XML 文档**解析成一个树形结构**，然后通过 JavaScript 等脚本语言来操作该文档的一种方式。DOM 树是由节点（节点包括元素、属性、文本等）构成的一种层次结构，开发者可以通过**操作 DOM 树的节点来改变文档的内容、结构和样式**等。DOM 树的根节点是 Document 对象，Document 对象提供了对文档的一些基本操作，例如获取和修改文档的标题、创建和删除节点等。

![image-20230220225614472](https://cdn.hcplantern.cn/img/2023/02/20/20230220-225616.png-default)

BOM 是指浏览器对象模型，它提供了访问浏览器窗口和浏览器本身的接口，例如窗口大小、历史记录、地址栏 URL 等。BOM 对象由多个对象组成，例如 window、navigator、history、screen 等，这些对象可以通过 JavaScript 等脚本语言来操作和控制。

DOM 和 BOM 是 Web 开发中不可或缺的两个概念。**DOM 用于操作文档的内容和结构**，而 **BOM 用于操作浏览器窗口和浏览器本身**。开发者可以通过 DOM 和 BOM 提供的接口，来创建交互式的 Web 应用程序，实现丰富的用户体验。

### 事件

JavaScript 事件是指**在 Web 页面中发生的各种交互和行为**，例如用户点击、鼠标移动、键盘输入等。当这些事件发生时，Web 浏览器会自动触发相应的事件处理函数，通常是由 JavaScript 编写的。JavaScript 事件处理程序可以通过监听事件来执行特定的代码，例如修改网页内容、调用其他函数、提交表单等。

JavaScript 中的事件通常由三个组成部分构成：

1. **事件源**：事件发生的对象，例如文本框、按钮等 HTML 元素。
2. **事件类型**：事件的类型，例如单击、双击、鼠标移动等。
3. **事件处理程序**：响应事件的代码块，通常由 JavaScript 编写。

#### 事件驱动编程

**事件驱动编程**是一种编程范式，它的核心思想是**应用程序的执行顺序由事件决定，而不是程序代码的顺序**。在事件驱动编程中，**应用程序通过监听事件来响应用户的操作和其他外部事件**，当事件发生时，应用程序会自动调用相应的事件处理函数来执行特定的代码逻辑，从而实现应用程序的功能。

事件驱动编程通常被用于开发 GUI（图形用户界面）应用程序和 Web 应用程序等需要用户交互的应用程序。在 GUI 应用程序中，事件驱动编程可以实现用户输入响应、按钮点击等操作，而在 Web 应用程序中，事件驱动编程可以实现用户单击链接、提交表单等操作。

事件驱动编程需要开发者设计合适的**事件模型和事件处理机制**，以及相应的**事件处理程序**，以实现应用程序的各种功能。通常，事件驱动编程还需要使用异步编程技术，以确保应用程序的响应速度和用户体验。

#### 事件处理模型

JavaScript 的事件处理模型是指浏览器在处理 Web 页面中发生的事件时所采用的一种机制，它包括事件的**捕获阶段、目标阶段和冒泡阶段**。当事件在页面中发生时，浏览器会首先在页面中的顶层对象（通常是 `window` 对象）上触发一个事件，然后按照一定的顺序依次触发每个包含事件目标元素的祖先元素上的事件，直到到达事件目标元素本身，最后再依次触发每个祖先元素的事件，这一过程被称为事件冒泡。

事件处理模型的三个阶段包括：

- 捕获阶段：事件从顶层对象开始往下捕获，直到达到目标元素的祖先元素为止。

- 目标阶段：事件到达目标元素。

- 冒泡阶段：事件从目标元素开始往上冒泡，直到达到顶层对象为止。

在事件处理模型中，每个阶段都可以设置事件处理函数。通常情况下，事件处理程序会在冒泡阶段进行处理。但是**在某些情况下，可以使用事件捕获来预先处理事件**，例如在验证用户输入等方面。通过对事件处理模型的掌握，开发者可以更好地理解和设计事件处理程序，提高 Web 应用程序的交互体验。

### JS 语言特性

#### 作用域

JavaScript 的作用域规定了变量和函数的可访问范围。在 JavaScript 中，作用域有两种：全局作用域和局部作用域。

1. 全局作用域： 全局作用域是指在代码中任何地方都可以访问的变量和函数，它们被定义在最外层的 JavaScript 文件或者 script 标签中，并且没有被包含在任何函数中。在全局作用域中声明的变量和函数都可以被其他作用域中的代码访问。
2. 局部作用域： 局部作用域是指变量或函数只在它们被定义的函数内部可见。在 JavaScript 中，每个函数都有自己的作用域，函数内部声明的变量或函数只在该函数内部可见，并且在函数外部是无法访问的。

在 ES6 中，引入了 **let 和 const 关键字来声明块级作用域变量**，它们的作用域仅限于当前的块中，包括 if、for、while 等语句块。这种作用域被称为块级作用域。

JavaScript 的**作用域链规定了在嵌套的函数中如何访问变量**。当在函数中访问变量时，如果当前函数的作用域中没有该变量，JavaScript 引擎会沿着作用域链一层层向上查找，直到找到该变量或者抵达全局作用域为止。

#### 暂时性死区

JavaScript 中的暂时性死区（Temporal Dead Zone，简称 TDZ）是指在代码块中，**在声明变量之前使用该变量会抛出错误**。在 ES6 中引入了 `let` 和 `const` 声明变量的方式，这种声明方式不会被提升（hoisting），而是在声明语句所在的块级作用域中创建变量，并且该变量存在一个 TDZ。在进入该作用域时，**从 TDZ 开始直到遇到该变量的声明语句为止，使用该变量都会抛出错误。**

```javascript
console. log (a); // ReferenceError: a is not defined
let a = 1;
```

上述代码中，变量 `a` 在使用之前就被访问了，因此会抛出 ReferenceError 错误。这是因为变量 `a` 存在于一个 TDZ 中，只有在 `let a = 1` 声明语句执行之后，`a` 变量才会被创建并分配内存。

#### let const var

在 JavaScript 中，let、const 和 var 都是用于声明变量的关键字，它们有以下区别：

1. 变量提升：var 关键字声明的变量存在变量提升，而 let 和 const 声明的变量不存在变量提升。
2. 作用域：var 声明的变量作用域是**函数作用域或全局作用域**，而 let 和 const 声明的变量作用域是**块级作用域**。
3. 可重复声明：var 关键字可以重复声明同一个变量，而 let 和 const 声明的变量在同一作用域内不能重复声明。
4. 初始值：var 关键字可以在声明变量时不初始化，而 let 和 const 必须**在声明时初始化**。
5. 可修改性：var 关键字和 let 关键字声明的变量可以修改，**const 关键字声明的变量不可修改**。

#### 闭包

闭包是指**一个函数可以访问它的外部函数作用域中的变量**，即使该外部函数已经返回了。具体来说，**闭包由两部分组成：一个函数和该函数所在的环境。环境是指闭包创建时的作用域链，其中包括当前作用域以及所有父级作用域。**函数可以访问该环境中的变量，即使这些变量在函数外部已经被销毁了。闭包在 JavaScript 中非常有用，因为它可以在**函数执行完毕后继续访问外部作用域中的变量**。这使得 JavaScript 开发人员能够编写更加模块化的代码，同时保护了内部变量不被外部代码篡改。

一个常见的例子是，在循环中使用闭包来创建多个回调函数，每个回调函数都可以访问它们所在的循环中的变量值。例如：

```js
function makeCounter() {
  var count = 0;
  return function() {
    count++;
    console.log(count);
  }
}

var counter = makeCounter();

for (var i = 0; i < 5; i++) {
  counter();
}

```

在这个示例中，`makeCounter` 函数返回一个内部函数，该函数可以访问 `makeCounter` 函数中定义的 `count` 变量。当调用 `counter` 变量时，实际上是调用 `makeCounter` 函数返回的内部函数，该函数会将 `count` 变量的值增加 1 并输出到控制台。

在 `for` 循环中，我们多次调用了 `counter` 变量，每次调用都会将 `count` 变量的值增加 1 并输出到控制台。但是，由于 `count` 变量是通过闭包实现的，因此**它不会被重新初始化为 0。相反，它会一直存在于内存中**，直到 `counter` 变量被销毁。

尽管闭包非常有用，但是它也有一些缺点。由于闭包保留了对外部环境的引用，因此如果闭包不正确使用，它**可能导致内存泄漏**。此外，在循环中使用闭包可能会导致性能问题，因为**每次迭代都会创建一个新的闭包**，增加了内存和计算开销。因此，在编写 JavaScript 代码时，需要谨慎使用闭包。

#### 提升

提升（Hoisting）是指在代码执行前，JavaScript 引擎会**将变量和函数的声明提升到作用域的顶部，但是初始化操作并不会被提升**。这意味着我们可以在声明变量或函数之前使用它们。

例如：

```js
foo(); // 输出 "Hello, World!"

function foo() {
  console.log("Hello, World!");
}

bar(); // 抛出 ReferenceError 异常

var bar = function() {
  console.log("Hello, World!");
}

```

在这个例子中，`foo()` 函数在它的声明之前被调用，但是由于**函数声明会被提升**，所以代码能够正常执行。相反，`bar()` 函数在它的声明之前被调用，但是由于**函数表达式不会被提升**，所以会抛出一个 `ReferenceError` 异常。

### 最佳实践正则原型对象 class this 框架的优缺点

#### 最佳实践

- 避免全局变量：使用局部变量替代、使用闭包

- 始终声明局部变量：局部变量必须通过 var 关键词来声明，否则它们将变成全局变量

- 使用严格模式：`use strict` ；保证代码运行安全；提高编译效率和运行速度；面向新版本 JS

  - 变量必须先声明再使用：在严格模式下，如果没有使用 var 或 let 声明变量，就会报错。

  - 禁止删除变量或函数：在严格模式下，使用 delete 删除变量或函数会抛出语法错误。

  - 禁止对只读属性赋值：在严格模式下，对只读属性进行赋值操作会抛出类型错误。

  - 函数的参数名不能重复：在严格模式下，函数的参数名不能重复。

  - 禁止使用 eval 和 arguments：在严格模式下，eval 和 arguments 被视为保留字，不能用作变量名或函数名。

  - 禁止使用 with 语句：在严格模式下，with 语句被禁用。

  - 启用严格模式的方式有两种，一种是在整个脚本文件的开头加上 "use strict" 语句，另一种是在函数内部的第一条语句中加上 "use strict" 语句。例如

  - ```js
    // 在脚本文件的开头启用严格模式
    "use strict";
    function myFunction() {
      // 在函数内部的第一条语句中启用严格模式
      "use strict";
      // 函数体
    }
    
    ```

  -  ES6 模块自动采用严格模式

- 在顶部声明：整洁代码；局部变量更集中；避免不需要的全局变量；减少重新声明的可能性 

- 初始化变量：避免未定义值

- 不要声明数值、字符串或布尔对象：将其视为原始值，加快执行速度、避免副作用

- 不要使用 `new Object()`：使用对象字面量（Object Literal）的方式来创建新对象，例如：

  - ```js
    const myObj = {
      name: 'John',
      age: 30,
      sayHello: function() {
        console.log('Hello');
      }
    };
    ```

  -  对象字面量的优点在于代码简洁、易于阅读和理解。此外，对象字面量还支持对象属性的简写方式，可以更加简化代码。

- 意识到⾃动类型转换

- 使⽤ `===` ⽐较：`==` ⽐较运算符总是在⽐较之前进⾏类型转换（以匹配类型）

- 使⽤ Parameter Defaults：如果调⽤函数时缺少⼀个参数，那么这个缺失参数的值会被设置为 undefined

- ⽤ default 来结束 switch

- 避免使⽤ eval ()：eval () 函数⽤于将⽂本作为代码来允许，存在安全问题

- 遍历对象内的成员时，你也会得到⽅法函数。为了解决这个问题，应始终将你的代码包装在⼀个 if 语句中来过滤信息。

  - ```js
    for(key in object) {
    	if(object.hasOwnProperty(key) {
    		...then do something...
    	}
    }
    ```

-  减少循环中的活动

#### VanillaJS

VanillaJS 是指使用原生（native）JavaScript 编写前端应用程序，而**不依赖于任何第三方库或框架的技术**。VanillaJS 通常被用来强调使用 JavaScript 原生能力进行开发的重要性，以及不一定需要使用大型框架来构建复杂的前端应用程序。VanillaJS 的优点包括：

1. 体积小：使用原生 JavaScript 代码，无需加载第三方库或框架的代码，因此整个应用程序的体积更小，可以更快地加载和渲染页面。
2. 性能更好：原生 JavaScript 的执行速度通常比使用框架或库的代码更快，可以提高应用程序的性能。
3. 更加灵活：使用原生 JavaScript 编写应用程序，可以更加灵活地处理各种问题和需求，不受框架或库的限制。
4. 更易维护：原生 JavaScript 代码通常比使用框架或库的代码更容易维护和更新，因为不需要考虑框架或库的特殊语法和规则。
5. 让你更好地掌握通用能力和生产力，更好地了解浏览器和计算及工作原理。

事实上，VanillaJS 并不是一个框架或者库，[称 VanillaJS 是一个“框架”或者“库”只是一种对盲目使用框架者的调侃](https://stackoverflow.com/questions/20435653/what-is-vanillajs)。

VanillaJs 的“官方网站”也十分有趣：[Vanilla JS (vanilla-js.com)](http://vanilla-js.com/)

当你想使用这种方式：`<script src="path/to/vanilla.js"></script>` 引入 vanillajs 的 script 时，你可以从官网下载 ~~然后发现 vanilla. js 其实是一个空文件~~

#### 正则表达式

#### 原型

在 JavaScript 中，对象有一个特殊的隐藏属性 `[[Prototype]]`，它要么为 `null`，要么就是对另一个对象的引用。该对象被称为“原型”。

当我们从 `object` 中读取一个缺失的属性时，JavaScript 会自动从原型中获取该属性。在编程中，这被称为“原型继承”。

#### class

在 ES6（ECMAScript 2015）之前，JavaScript 中没有 class 关键字，而是**通过构造函数和原型链实现面向对象编程**。而在 ES6 中，class 被引入作为一个新的语法糖，以更加清晰和简洁的方式来定义对象和继承关系，其背后的实现机制仍然是**使用构造函数和原型链**。通过 class 关键字，可以定义一个类，这个类可以包含构造函数、类方法和类属性等内容。通过 class 关键字，我们还可以实现类的继承。例：

```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  sayHi() {
    console.log(`Hi, my name is ${this.name} and I am ${this.age} years old.`);
  }
}

class Student extends Person {
  constructor(name, age, grade) {
    super(name, age);
    this.grade = grade;
  }
  sayHi() {
    console.log(`Hi, my name is ${this.name}, I am ${this.age} years old, and I am in grade ${this.grade}.`);
  }
}

let john = new Person('John', 25);
john.sayHi(); // Hi, my name is John and I am 25 years old.

let jane = new Student('Jane', 17, 12);
jane.sayHi(); // Hi, my name is Jane, I am 17 years old, and I am in grade 12.
```

在这个例子中，我们定义了一个 Student 类，它继承了 Person 类。我们使用 super 关键字来调用父类的构造函数，同时在子类中定义了一个新的属性 grade 和一个重写的方法 sayHi。

#### this

在 JavaScript 中，`this` 关键字指向当前函数执行的上下文。具体来说，它指向当前执行函数的对象，也就是函数被调用时的上下文对象。

`this` 的值可以根据调用方式和执行环境而变化。在函数体中，⾮显式或隐式地简单调⽤函数时，在严格模式下，函数内的 this 会被绑定到 undefined 上，在⾮严格模式下则会被绑定到全局对象 window/global 上

在对象中调用方法时，`this` 指向该对象；

在构造函数中调用时，`this` 指向实例化出来的对象。

例子：

```js
function f1() {
 console.log(this);
}
function f2() {
 "use strict";
 console.log(this);
}
f1(); // window
f2(); // undefined

var foo = {
 bar: 10,
 fn: function() {
 console.log(this)
 console.log(this.bar)
 }
}
var fn1 = foo.fn
fn1() // window, undefined

var foo = {
 bar: 10,
 fn: function() {
 console.log(this)
 console.log(this.bar)
 }
}
// this指向最后调⽤它的对象
foo.fn() // {bar: 10, fn: f}, 10

var student = {
 name: 'Lucas',
 fn: function() {
 return this
 }
}
console.log(student.fn() === student) // true

var person = {
	name: 'Lucas',
	brother: {
		name: 'Mike',
		fn: function() {
			return this.name
    	}
   	}
}
// this 指向 brother
console.log(person.brother.fn()) // Mike

var o1 = {
 text: 'o1',
 fn: function() {
 return this.text
 } }
var o2 = {
 text: 'o2',
 fn: function() {
 return o1.fn()
 } }
var o3 = {
 text: 'o3',
 fn: function() {
 var fn = o1.fn
 return fn()
 } }
console.log(o1.fn()) // o1
console.log(o2.fn()) // o1
console.log(o3.fn()) // undefined
```



`this` 的值在函数定义时是不确定的，只有在函数执行时才能确定。因此，在回调函数、定时器和事件处理程序等场景中，`this` 的值可能会出现意外的情况，需要进行特殊处理。

在 ES6 中，箭头函数的 `this` 值是在函数定义时确定的，它的值与外层作用域中的 `this` 值保持一致。这使得箭头函数更加方便和易于理解。

```js
const foo = { 
 fn: function(){ 
 setTimeout(function() { 
 console.log(this)
 })
 } 
} 
console.log(foo.fn()) // window
```

```js
var foo = { 
 fn: function(){ 
 setTimeout(() => { 
 console.log(this)
 })
 } 
} 
console.log(foo.fn()) // {fn: f}
```

#### Babel

Babel 是一个 JavaScript 编译器，可以将 ECMAScript 2015+ 代码转换为向后兼容的 JavaScript 版本。

Babel 支持各种语言特性，例如箭头函数、类、解构、默认参数、rest 参数、let、const、模板字符串等等。Babel 还支持各种插件和预设，可以扩展其功能，例如使用 TypeScript、Flow 等等。

## 前端框架

前端框架可以帮助开发者更高效地构建 Web 应用程序，主要包括以下几种类型：

1. MVVM 框架：Vue. js、Angular、React 等
2. UI 框架：Bootstrap、Ant Design、Element UI、Material UI 等
3. 数据可视化框架：ECharts、D3. js、Highcharts 等
4. 工具类框架：jQuery、Lodash、Underscore 等

使用前端框架的主要优势包括：

1. 提高开发效率：框架提供了一系列可复用的组件和工具，减少了开发时间和成本。
2. 优化性能：框架经过优化，能够更快速地渲染页面，提高用户体验。
3. 易于维护：框架具有良好的结构和组织方式，易于维护和升级。
4. 社区支持：框架拥有庞大的社区，提供丰富的文档和示例，能够帮助开发者快速解决问题。

在进行框架选型时，应该考虑以下几个方面：

1. 项目需求：根据项目需求来选择适合的框架，例如需要实现复杂数据交互的项目可以选择 Angular，需要快速构建单页面应用的项目可以选择 Vue. js 等。
2. 开发人员技能：开发团队的技能水平也需要考虑，选择开发人员熟悉的框架能够提高开发效率。
3. 性能需求：不同的框架对性能的影响不同，需要根据项目性能需求进行选择。
4. 社区支持：框架的社区支持能够提供丰富的资源和帮助，需要考虑框架的社区规模和活跃度。

在框架选型后，需要进行框架的学习和熟悉，以便能够充分利用框架的优势，并且在开发过程中不断完善和优化代码。

## Nodejs

### 客户端与服务端编程

客户端编程和服务端编程是两种不同的计算机编程方式，分别用于开发客户端应用程序和服务器端应用程序。

客户端编程指的是**开发运行在用户终端设备（如电脑、手机、平板等）上的应用程序**。客户端应用程序通过与服务器通信来获取和发送数据。客户端编程通常使用**HTML、CSS 和 JavaScript**等前端技术，开发 Web 页面和 Web 应用、移动应用程序和桌面应用程序等。

服务端编程则指的是**开发运行在服务器上的应用程序**。服务端应用程序主要用于处理客户端应用程序发送的请求和提供数据和服务。服务端编程通常使用**后端编程语言和框架，例如 Java、Python、PHP、Ruby、Node. js**等。

客户端编程和服务端编程经常需要配合使用，以提供完整的应用程序。客户端与服务端之间通过网络通信来实现数据的交换和服务的提供。

### 静态网页和动态网页

静态网页和动态网页是基于网页生成方式的两种不同类型。

静态网页指的是**内容不变的网页，通常是由 HTML 文件和 CSS 样式表构成**的，页面上的内容都是固定的，不会随着用户的行为而改变。静态网页的生成方式通常是**在服务器端生成好 HTML 文件，然后将文件发送给客户端浏览器进行渲染**。

动态网页指的是内容随着用户行为或外部数据的改变而改变的网页。动态网页的生成方式是**在服务器端根据用户请求或外部数据的变化动态生成 HTML 文件**。动态网页通常需要**使用服务器端脚本语言**，如 PHP、Python、Node. js 等，来实现动态生成 HTML 文件的功能。

相对于静态网页，动态网页具有更好的**交互性和实时性**，能够根据用户行为或外部数据的变化动态更新页面内容，但也需要更多的服务器资源和技术支持。

### Node. js 特点

Node. js 是一个基于事件驱动的非阻塞 I/O 模型的 JavaScript 运行环境。以下是 Node. js 的主要特点：

1. 事件驱动：Node. js 的核心是事件循环机制，通过监听事件来调用回调函数，从而实现非阻塞 I/O 操作。
2. 异步非阻塞 I/O 模型：Node. js 通过异步非阻塞 I/O 操作来提高性能，避免了传统阻塞式 I/O 操作的等待时间，提高了应用程序的响应速度和吞吐量。
3. 单线程模型：Node. js 采用单线程模型，通过事件循环机制和异步非阻塞 I/O 操作来实现高性能。
4. 轻量级：Node. js 的设计初衷是轻量级的，因此它的安装包相对较小，开发者可以快速地下载和安装，也非常适合构建小型应用程序和微服务。
5. 跨平台：Node. js 可以在多个平台上运行，包括 Windows、Linux、macOS 等。
6. 社区活跃：Node. js 有着庞大的社区支持，开源社区成员积极参与开源项目的维护和更新，提供了大量的开源模块和库，使得开发人员可以快速地构建应用程序。
7. 适合实时应用：Node. js 非常适合构建实时应用程序，如在线游戏、即时通讯、实时数据分析等。
8. 适合前端开发人员：Node. js 使用 JavaScript 语言，这使得前端开发人员可以用相同的技术栈开发后端应用程序，提高了开发效率和代码复用性。
9. 易于部署：Node. js 应用程序可以通过简单的命令行工具来部署，这使得应用程序的部署和维护变得更加简单和高效。

### Node. js 应用

- ⽹站（如 express/koa 等）
- im 即时聊天 (socket. io) 
- api（移动端，pc，h5） 
- HTTP Proxy（淘宝、Qunar、腾讯、百度都有） 
- 前端构建⼯具 (grunt/gulp/bower/webpack/fis3…) 
- 写操作系统（NodeOS） 
- 跨平台打包⼯具（PC 端的 electron、nw. js，⽐如钉钉 PC 客户端、微信⼩程序 IDE、微信客户端，移动的 cordova，即⽼的 Phonegap，还有更加有名的⼀站式开发框架 ionicframework） 
- 命令⾏⼯具（⽐如 cordova、shell. js） 
- 反向代理（⽐如 anyproxy，node-http-proxy）

### Node. js 优势和缺点

优势：

- 高性能： Node. js 的事件驱动、非阻塞 I/O 模型使得它非常适合处理高并发、低延迟的应用程序。
- 轻量级： Node. js 的设计初衷是轻量级的，它非常适合构建小型应用程序和微服务。
- 前后端一致性： Node. js 的出现使得前后端可以使用同一种语言（JS）和技术栈开发，从而提高了开发效率和代码复用性。
- 语言理念更新：与传统的 web 服务器语⾔（例如 Python、PHP 等）相⽐，JavaScript 理念更新，语⾔设计的改进带来了诸多好处。许多其它新近流⾏的语⾔也可编译/转换成 JavaScript，所以 TypeScript、CoffeeScript、ClojureScript、Scala、LiveScript 等等也可以使⽤。
- Node 包管理工具：自动化工具链构建
- 可移植性
- 社区活跃： Node. js 有着非常庞大的社区，社区成员积极参与开源项目的维护和更新，因此，Node. js 拥有丰富的第三方库和插件，可以快速完成开发任务。

缺点：

1. 单线程： Node. js 的单线程模型意味着它**不能利用多核处理器**的优势，如果**应用程序需要大量计算和 CPU 密集型操作**，例如视频编码和图像处理，性能可能会受到影响。
2. 内存泄漏： Node. js 的内存管理是手动的，如果应用程序没有正确地处理**内存泄漏**，可能会导致应用程序崩溃或性能下降。
3. 异步编程： Node. js 使用异步编程模型，这使得代码的编写和调试都比较困难，需要开发人员具备一定的编程经验和技巧。
4. 缺乏标准库： Node. js 的标准库相对较小，需要依赖第三方模块来完成许多常见任务，这使得开发人员需要花费额外的时间来搜索和使用这些模块。

### 事件驱动模型

![image-20230221165252259](https://cdn.hcplantern.cn/img/2023/02/21/20230221-165254.png-default)

- Application 应用层，即 JavaScript 交互层，常见的就是 Node. js 的模块，比如 http，fs 等
- V8 这一层是 V8 引擎层，这一层的主要作用是解析 JavaScript，同时和应用层和 NodeApi 层交互
- NodeApi 为上层模块提供系统调用，和操作系统进行交互
- Libuv 是跨平台的底层封装，实现了线程池、事件循环、文件操作等，是 Node. js 实现异步的核心 

在 Libuv 层维护了一个 Event Queue 的事件队列，当有请求过来时，经过 Node. js 的应用层和 NodeApi 层将请求作为一个事件放到 Event Queue 事件队列中，并设置回调事件函数，然后继续接受新的请求。

在 Libuv 层的 Event Loop 事件循环不断读 Event Queue 中的事件，在读取事件的过程中如果遇到非阻塞事件，会自已处理，并且在处理完后调用回调函数向上一层返回结果；对于阻塞事件，会委托给后台线程池来处理，当这些阻塞操作完成后，**执行结果与提供的回调函数一起再被放入事件队列中**。当 Event Loop 再次读到这个事件时，会再次执行被放到队列中的事件回调函数，最后将结果返回给上一层。

### 同步 IO 与异步 IO

**同步和异步其实指的是，请求发起方对消息结果的获取是主动发起的，还是等被动通知的。**

如果是请求方主动发起的，一直在等待应答结果（同步阻塞），或者可以先去处理其他的事情，但要不断轮询查看发起的请求是否有应答结果（同步非阻塞 ）因为不管如何都要发起方主动获取消息结果，所以形式上还是同步操作。

如果是由服务方通知的，也就是请求方发出请求后，要么在一直等待通知（异步阻塞），要么就先去干自己的事了（异步非阻塞），当事情处理完成之后，服务方会主动通知请求方，它的请求已经完成，这就是异步。异步通知的方式一般是通过状态改变，消息通知，或者回调函数来完成，大多数时候采用的都是回调函数。

| 同步式 I/O （阻塞式）               | 异步式 I/O （非阻塞式）   |
| ----------------------------------- | ------------------------- |
| 利用多线程提供吞吐量                | 单线程即可提高吞吐量      |
| 通过事件片分割和线程调度利用多核 CPU | 通过功能划分利用多核 CPU   |
| 需要由操作系统调度多线程使用多核 CPU | 可以将单进程绑定到单核 CPU |
| 难以充分利用 CPU 资源                 | 可以充分利用 CPU 资源       |
| 内存轨迹大，数据局部性弱            | 内存轨迹小，数据局部性强  |
| 符合线性的编程思维                  | 不符合传统编程思维        |

### 阻塞与非阻塞

阻塞和非阻塞在计算机的世界里面，通常指的是针对 IO 的操作，如网络 IO 和磁盘 IO 等。

当调用了一个函数之后，在等待这个函数返回结果之前如果是挂起状态，就意味着当前线程**什么都不能干，就等着获取结果**，这就叫阻塞；如果仍然是运行状态，就意味当前线程是**可以继续处理其他任务**，这是非阻塞。

### 模块

Node. js 的模块是一种组织和管理代码的方式，它能够将代码分解成可重用的、独立的部分，并将它们组合起来以创建复杂的应用程序。

- 模块化机制：Node. js 使用 CommonJS 规范来实现模块化，每个模块都是一个独立的文件，可以通过 require () 函数来引用其他模块，也可以通过 module. exports 来导出当前模块的接口。
- 内置模块：Node. js 内置了很多常用的模块，如 fs 模块用于文件操作、http 模块用于处理 HTTP 请求、crypto 模块用于加密等等。
- 第三方模块：Node. js 的第三方模块非常丰富，可以通过 npm（Node Package Manager）来安装和管理第三方模块，包括 Express、Socket. IO、Mongoose 等等。
- 自定义模块：除了使用内置模块和第三方模块外，开发者还可以根据自己的需求编写自定义模块，并将其导入到应用程序中使用。

### ES6 和 CommonJS

|              | ES6                                              | CommonJS                           |
| ------------ | ------------------------------------------------ | ---------------------------------- |
| 语法         | 使用 import/export 语法来导入和导出模块            | 使用 require/module. exports         |
| 解析         | 静态解析，可以在编译时确定模块的依赖关系         | 动态加载，需要在运行时确定依赖关系 |
| 导入/导出    | 输出值的引用，导入的变量是只读的                 | 输出值的拷贝，导入的变量是可变的   |
| 浏览器兼容性 | 原生支持，需要在浏览器中使用时需要进行编译和打包 | 需要借助工具才能在浏览器中使用     |
| 模块加载     | 异步加载                                         | 同步加载                           |



CommonJS 输出的是值的拷贝，换句话说就是，一旦输出了某个值，如果模块内部后续的变化，影响不了外部对这个值的使用。具体例子：

```javascript
// lib.js
var counter = 3;
function incCounter() {
  counter++;
}
module.exports = {
  counter: counter,
  incCounter: incCounter,
};
```

然后我们在其它文件中使用这个模块：

```js
var mod = require('./lib');
console.log(mod.counter);  // 3
mod.incCounter();
console.log(mod.counter); // 3
```

ES6 模块运行机制完全不一样，JS 引擎对脚本静态分析的时候，遇到模块加载命令 `import`，就会生成一个只读引用。等到脚本真正执行的时候，再根据这个只读引用，到被加载的那个模块里去取值。

```js
// lib.js
export let counter = 3;
export function incCounter() {
  counter++;
}

// main.js
import { counter, incCounter } from './lib';
console.log(counter); // 3
incCounter();
console.log(counter); // 4
```

上面代码说明，ES6 模块 `import` 的变量 `counter` 是可变的，完全反应其所在模块 `lib.js` 内部的变化。

### 循环依赖

CommonJS 模块的特性就是加载时执行，当脚本被 `reqiure` 的时候，就会全部执行。一旦出现某个模块被"循环加载"，就只输出已经执行的部分，还未执行的部分不会输出。

```js
// a.js
exports.done = false;
var b = require('./b.js');
console.log('在 a.js 之中，b.done = %j', b.done);
exports.done = true;
console.log('a.js 执行完毕');

// b.js
exports.done = false;
var a = require('./a.js');
console.log('在 b.js 之中，a.done = %j', a.done);
exports.done = true;
console.log('b.js 执行完毕');

// main.js
ar a = require('./a.js');
var b = require('./b.js');
console.log('在 main.js 之中, a.done=%j, b.done=%j', a.done, b.done);

// 输出结果
在 b.js 之中，a.done = false
b.js 执行完毕
在 a.js 之中，b.done = true
a.js 执行完毕
在 main.js 之中, a.done=true, b.done=true
```

ES6 模块是动态引用，如果使用 `import` 加载一个变量，变量不会被缓存，真正取值的时候就能取到最终的值。

```js
// even.js
import { odd } from './odd'
export var counter = 0;
export function even(n) {
  counter++;
  return n === 0 || odd(n - 1);
}

// odd.js
import { even } from './even';
export function odd(n) {
  return n !== 0 && even(n - 1);
}
```

输出值：

```shell
> import * as m from './even.js';
> m.even(10);
true
> m.counter
6
```

上面代码中，参数 `n` 从 10 变为 0 的过程中，`even()` 一共会执行 6 次，所以变量 `counter` 等于 6。在这个例子中，我们可以看到，`even.js` 中输出的 `counter` 变量值会随着模块内部的变化而变化。

### 回调地狱

回调地狱（Callback Hell）是指在**使用异步编程**时，**多个嵌套的回调函数形成的一种代码结构，难以维护和阅读**。在 Node. js 中，由于许多操作是异步的，例如读写文件、处理 HTTP 请求等，因此经常需要使用回调函数来处理异步操作的结果。如果嵌套的回调函数过多，代码就会变得难以阅读和维护，称为回调地狱。

以下是一个回调地狱的示例：

```js
fs.readFile('file1.txt', function (err, data) {
  if (err) {
    console.error(err);
  } else {
    fs.readFile('file2.txt', function (err, data) {
      if (err) {
        console.error(err);
      } else {
        fs.writeFile('file3.txt', data, function (err) {
          if (err) {
            console.error(err);
          } else {
            console.log('All done!');
          }
        });
      }
    });
  }
});
```

可以看到，每个异步操作都需要嵌套一个回调函数，导致代码缩进不断增加，可读性非常差，也难以维护。

解决方法： 

- 使用 Promise、async/await 等技术来改善异步编程的代码结构。
- 拆解 funciton，定义函数而不是就地定义回调。

## Express

Express 是一个流行的 Node. js Web 框架，它提供了一系列的 API 和中间件，使得构建 Web 应用程序变得简单和灵活。

- 快速搭建：Express 提供了简单易用的 API 和中间件，可以快速搭建 Web 应用程序，减少了开发的时间和难度。
- **路由管理**：Express 提供了路由管理功能，可以根据 HTTP 请求的 URL 和方法将请求分发到对应的处理函数中。
- **中间件支持**：Express 中间件机制非常强大，可以方便地添加、删除和修改中间件，以及自定义中间件来扩展应用程序的功能。
- 视图引擎：Express 支持多种视图引擎，如 EJS、Pug 和 Handlebars 等，可以方便地渲染动态页面。
- **错误处理**：Express 提供了错误处理机制，可以方便地处理和捕获应用程序中的错误，并给出相应的响应。
- 数据库支持：Express 不直接提供数据库支持，但可以轻松地与多种数据库集成，如 MongoDB、MySQL、PostgreSQL 等。
- 扩展性强：Express 是一个灵活的框架，可以自由地组合和扩展中间件，以满足应用程序的需求。

![image-20230222134317286](https://cdn.hcplantern.cn/img/2023/02/22/20230222-134318.png-default)

- 路由：把需要⽀持的请求（以及请求 URL 中包含的任何信息）转发到适当的控制器函数。 
- 控制器：从模型中获取请求的数据，创建⼀个 HTML ⻚⾯显示出数据，并将⻚⾯返回给⽤户，以便在浏览器中查看。 
- 视图（模板）：供控制器⽤来渲染数据。

### 路由

在 Express 中，路由规则是用来定义客户端请求 URL 和服务器端响应之间的映射关系的。路由规则的定义可以通过 Express 应用程序对象 app 的 HTTP 请求方法来完成，如 app. get ()、app. post ()、app. put ()、app. delete () 等。

下面是一些常见的 Express 路由规则：

定义基本路由规则

```js
app.get('/', function(req, res) {
  res.send('Hello World');
});

app.post('/', function(req, res) {
  res.send('Got a POST request');
});
```

定义带参数的路由规则

```js
app.get('/users/:id', function(req, res) {
  res.send('The user id is: ' + req.params.id);
});

app.get('/users/:id/books/:bookId', function(req, res) {
  res.send('The user id is: ' + req.params.id + ' and the book id is: ' + req.params.bookId);
});
```

定义带查询参数的路由规则

```js
app.get('/users', function(req, res) {
  res.send('The user name is: ' + req.query.name);
});
```

使用正则表达式匹配路由规则

```js
app.get(/^\/users\/(\d+)$/, function(req, res) {
  res.send('The user id is: ' + req.params[0]);
});
```

使用路由中间件来处理请求

```js
var myLogger = function (req, res, next) {
	console.log('LOGGED');
	next();
};
app.use(myLogger);
app.get('/', function (req, res) {
	res.send('Hello World!');
});
```

使用 Express Router 来模块化路由规则

```js
// users.js
var express = require('express');
var router = express.Router();

router.get('/', function(req, res) {
  res.send('Users home page');
});

router.get('/profile', function(req, res) {
  res.send('Users profile page');
});

module.exports = router;

// app.js
var usersRouter = require('./users');
app.use('/users', usersRouter);
```



### 中间件

中间件是⼀种**功能的封装⽅式**，具体来说就是封装在程序中处理 HTTP 请求的功能。中间件是在管道中执⾏的。在 Express 程序中，通过调⽤ app. use 向管道中插⼊中间件。

```js
var app = express();
var router = express.Router();
// a middleware function with no mount path. This code is executed for every request to the router
router.use(function (req, res, next) {
 	console.log('Time:', Date.now());
	next();
});
// a middleware sub-stack shows request info for any type of HTTP request to the /user/:id path
router.use('/user/:id', function(req, res, next) {
 	console.log('Request URL:', req.originalUrl);
 	next();
}, function (req, res, next) {
 	console.log('Request Type:', req.method);
 	next();
});
// a middleware sub-stack that handles GET requests to the /user/:id path
router.get('/user/:id', function (req, res, next) {
 	// if the user ID is 0, skip to the next router
 	if (req.params.id == 0) next('route');
 	// otherwise pass control to the next middleware function in this stack
 	else next(); //
}, function (req, res, next) {
 	// render a regular page
 	res.render('regular');
});
// handler for the /user/:id path, which renders a special page
router.get('/user/:id', function (req, res, next) {
 	console.log(req.params.id);
 	res.render('special');
});
// mount the router on the app
app.use('/', router);

```

next ('route') 跳过过路由器中间件堆栈中剩余的中间件函数，将控制权传递给下一个路由

#### 错误处理中间件

错误处理中间件是一种特殊的中间件，用于捕获应用程序中的错误并做出响应。它通常是在所有其他中间件函数之后定义的，用于处理任何未被捕获的错误。

错误处理中间件函数通常接收四个参数：err、req、res、next。其中，err 是错误对象，req 和 res 是请求和响应对象，next 是下一个中间件函数的引用。**如果错误处理中间件函数没有传递 err 参数，则 Express 将默认该中间件为普通中间件函数。**

Express 中的错误处理中间件有两种形式：

1. 错误处理中间件函数

```js
app.use(function(err, req, res, next) {
  console.error(err.stack);
  res.status(500).send('Something broke!');
});
```

上述代码定义了一个错误处理中间件函数，用于**处理所有未被捕获的错误**。当应用程序发生错误时，该中间件将打印错误堆栈并向客户端发送 HTTP 500 错误响应。

1. 错误处理中间件工厂函数

```js
function errorHandler(err, req, res, next) {
  console.error(err.stack);
  res.status(500).send('Something broke!');
}

app.use(errorHandler);
```

上述代码定义了一个错误处理中间件工厂函数，该函数返回一个错误处理中间件函数。可以将该函数传递给 app. use () 方法，以将其作为错误处理中间件使用。

如果错误处理中间件函数在调用时抛出了异常，则该异常将被 Express 捕获，并将其作为 500 HTTP 错误发送到客户端。

#### 内置中间件

Express 中内置了大量的中间件，如 body-parser、cookie-parser、morgan、serve-static 等。此外，还可以使用第三方中间件，如 passport、multer 等。

可以通过组合多个中间件函数来构建复杂的请求处理流程，例如：

```js
app.use(express.static('public'));

app.use('/users', function(req, res, next) {
  console.log('Request URL:', req.originalUrl);
  next();
});

app.get('/users', function(req, res) {
  res.send('Users home page');
});
```

在上述代码中，首先使用 express. static () 中间件来处理静态文件请求，然后使用带路径的中间件函数来处理 URL 以'/users'开头的请求，最后使用普通中间件函数来处理 GET 请求。



### 模板

Express 模板引擎是一种将数据与模板结合起来生成 HTML 的工具。使用模板引擎，我们可以将 HTML 代码分离出来，将其存储在一个单独的文件中，然后通过编写动态内容的代码将数据注入到模板中，以生成最终的 HTML 响应。这种技术能够大大简化 HTML 代码的编写和维护，使应用程序更加灵活和易于开发。

Express 支持多种模板引擎，包括 EJS、Pug（原名 Jade）、Handlebars 等。每种模板引擎都有其独特的语法和用法，但它们都支持以下基本功能：

1. 变量插值：在模板中嵌入变量，如 `<h1>Hello, <%= name %>!</h1>`。
2. 条件语句：根据条件来控制内容的输出，如 `<% if (user.isAdmin) { %><p>Welcome, admin!</p><% } %> `。
3. 循环语句：重复输出某些内容，如 `<% users.forEach(function(user) { %><li><%= user.name %></li><% }) %>`。

在 Express 中，使用模板引擎需要完成以下步骤：

1. 安装模板引擎：可以使用 npm 安装所需的模板引擎，如 `npm install ejs --save`。
2. 配置 Express 应用程序：需要告诉 Express 使用哪种模板引擎以及模板文件的存放路径，如：

```js
const express = require('express');
const app = express();

app.set('view engine', 'ejs');
app.set('views', __dirname + '/views');
```

3. 创建模板文件：在指定的模板文件夹中创建 HTML 模板文件，如 `views/index.ejs`。
4. 创建路由：编写 Express 路由函数，在函数中渲染模板并将其作为响应发送，如：

```js
app.get('/', (req, res) => {
  const name = 'John Doe';
  res.render('index', { name });
});
```

上述代码通过调用 `res.render()` 方法渲染名为 `index.ejs` 的模板，并将数据{name: 'John Doe'}传递给模板。渲染后的 HTML 响应将发送给客户端。

## Ajax （不用花太多时间）

AJAX 允许只更新⼀个 HTML ⻚⾯的部分 DOM，⽽⽆须重新加载整个⻚⾯。AJAX 还允许异步⼯作，这意味着当⽹⻚的⼀部分正试图重新加载时，您的代码可以继续运⾏（相⽐之下，同步会阻⽌代码继续运⾏，直到这部分的⽹⻚完成重新加载）。

优点：

- 提高用户体验：Ajax 可以在不刷新整个页面的情况下更新部分页面内容，从而提高用户的**交互体验和响应速度**。
- 减少服务器负载：Ajax 可以避免频繁的页面刷新，**减轻服务器的负担**。
- 降低带宽消耗：Ajax 只传输需要更新的数据，而不是整个页面，从而**减少了带宽的消耗**。
- 可以异步加载数据：Ajax 可以异步加载数据，从而使得用户在等待数据加载时可以继续浏览页面或进行其他操作。

缺点：

- 对搜索引擎不友好：由于 Ajax 在页面不刷新的情况下更新部分页面内容，因此搜索引擎无法抓取整个页面的内容，从而**降低了 SEO 效果**。
- **前进/后退功能**不可用：Ajax 更新部分页面内容，而浏览器的前进/后退功能是基于整个页面的，因此在使用 Ajax 时需要特殊处理。
- 安全性问题：由于 Ajax 的异步请求不会受到同源策略的限制，因此如果不适当的处理跨站脚本攻击，会存在安全性问题。
- 可维护性问题：由于 Ajax 技术使得代码逻辑分散在多个地方，因此可能会导致代码的可维护性问题。

## HTTP 1/2/3

演化路线：

1. HTTP/0.9：该版本于 1991 年推出，只支持 GET 方法，只能传输 HTML 格式的文本，没有 Header 信息和状态码，也没有 Cookie 和 Session 等机制。
2. HTTP/1.0：该版本于 1996 年推出，支持多种 HTTP 方法，如 GET、POST、PUT、DELETE 等，支持 Header 信息、状态码、Cache 等机制，但没有持久连接和流水线等机制。
3. HTTP/1.1：该版本于 1999 年推出，增加了持久连接和流水线等机制，支持压缩和分块传输编码等功能，也增加了 Host 头字段和 Range 头字段等新的 Header 字段。
4. HTTP/2：该版本于 2015 年推出，通过使用二进制协议、多路复用、头部压缩等技术，实现了更高效的传输，减少了延迟和网络负载，同时还支持服务器推送等新的功能。
5. HTTP/3：该版本于 2020 年推出，基于 QUIC 协议，通过减少握手次数、降低重传时间、提高连接可靠性等优化，实现了更快的连接速度和更高的传输效率。

![image-20230222153414010](https://cdn.hcplantern.cn/img/2023/02/22/20230222-153415.png-default)

![image-20230222154330083](https://cdn.hcplantern.cn/img/2023/02/22/20230222-154331.png-default)

### HTTP 1.1

1. **持久连接**：允许在同一连接上发送多个请求和响应，从而避免了每次请求都需要重新建立连接的开销。
2. **流水线**：在同一连接上同时发送多个请求，从而避免了请求和响应之间的等待时间。
3. 应用层**分片**技术：将 HTTP 请求和响应分割成多个小片段进行传输的技术，可以提高 HTTP 协议的传输效率和性能。
4. **压缩**：内容压缩如 gzip 和 deflate，减小了传输的数据量。
5. **缓存控制**：缓存控制如 Cache-Control 和 Expires，从而减少了不必要的网络传输。

然而：无法压缩 Header 信息

### HTTP 2

HTTP/2 采用了新的**二进制传输协议**，以取代 HTTP/1. x 协议中的文本格式传输（例如，HTTP 头和请求/响应体使用明文传输）。二进制传输协议可以将数据编码为二进制格式，并使用更有效的压缩算法和数据结构来传输数据。HTTP/2 使用二进制协议分帧，将 HTTP 请求和响应分割成多个小帧进行传输，同时实现多路复用。

1. **二进制协议和多路复用**：HTTP/2 使用二进制协议，支持多路复用，即在同一连接上同时发送多个请求和响应，从而避免了队头阻塞和连接建立的开销。
2. **流量控制**：HTTP/2 使用流量控制机制，即通过控制每个流的窗口大小，从而避免了拥塞和流的阻塞。
3. **头部压缩**：HTTP/2 使用 HPACK 算法对头部进行压缩，从而减小了传输的数据量。
4. **服务器推送**：HTTP/2 支持服务器推送，即在客户端请求前，服务器主动推送相关资源到客户端缓存中，从而减少了请求和等待时间。

### HTTP 1.1 流水线和 HTTP 2 多路复用

HTTP/1.1 的流水线技术允许客户端在发送请求之前不需要等待服务器响应，可以同时发送多个请求。服务器也可以按照请求顺序依次处理多个请求，提高响应速度和效率。然而，由于 HTTP/1.1**采用了文本格式传输**，且请求和响应都是按照顺序传输，当出现某个请求**阻塞或延迟**时，会阻塞整个连接，影响性能。

HTTP/2 采用了新的二进制传输协议和多路复用技术，可以将多个请求和响应分割成多个小帧进行传输，通过同一个连接**并行传输多个流**，避免了 HTTP/1.1 中阻塞整个连接的问题，提高了性能和效率。多路复用技术可以实现更好的资源利用和更快的页面加载速度。

### HTTP 3

1. 基于**UDP 协议**实现：HTTP/3 采用 UDP 协议作为底层传输协议，不再使用 TCP 协议，这样可以减少网络延迟和丢包的问题。
2. 基于**QUIC 协议**实现：HTTP/3 使用 QUIC 协议作为传输层协议，QUIC 协议具有流量控制和多路复用等优秀特性，可以显著提高网络传输的效率和性能。
3. 0-RTT 握手：HTTP/3 支持 0-RTT 握手，即在客户端和服务器之间建立连接时，可以省略一部分握手过程，从而提高连接的建立速度。
4. HTTP 头部压缩优化：HTTP/3 采用基于 QPACK 算法的头部压缩，能够更好地处理头部信息，减少传输数据量。
5. 服务器推送优化：HTTP/3 的服务器推送能力也得到了优化，能够更好地提供资源推送服务。
6. 连接迁移：HTTP/3 支持连接迁移，即在客户端和服务器之间切换传输路径时，可以保持连接状态和传输性能。

### HTTPS

HTTPS 是基于 HTTP 协议的安全协议，特点：

1. 数据加密：HTTPS 使用 SSL/TLS 协议对数据进行加密传输，可以防止数据在传输过程中被窃听、篡改或伪造，确保数据传输的安全性。
2. 身份认证：HTTPS 使用数字证书对网站的身份进行认证，防止网站被冒充或伪造，确保用户访问的是真正的网站，提高数据传输的可信度和安全性。
3. SEO 优化：搜索引擎更倾向于显示和收录 HTTPS 网站的内容，HTTPS 网站的排名更容易优化和提高。
4. 网站速度：HTTPS 网站相比 HTTP 网站，传输数据需要进行加密和解密操作，会增加一定的计算负担和传输延迟，从而可能会导致网站速度的下降。
5. 成本：HTTPS 网站需要购买数字证书，增加了网站的运营成本。

### 请求方法

1. GET：获取资源，可以理解为读取服务器资源。请求参数放在 URL 的查询参数中。
2. POST：提交数据，可以理解为向服务器写入数据。请求参数放在请求体中。
3. PUT：更新资源，与 POST 的区别在于 PUT 一般是对完整资源的更新，而 POST 是对资源进行修改或追加。
4. DELETE：删除资源。
5. HEAD：获取资源的元数据，与 GET 的区别在于 HEAD 只返回资源的元数据信息，而不返回资源本身。
6. OPTIONS：获取服务器支持的 HTTP 请求方法，服务器会返回支持的 HTTP 请求方法、服务器支持的各种头信息等。
7. TRACE：追踪请求-响应的传输路径，用于诊断，由于安全原因，现在一般都会禁用 TRACE 方法。
8. CONNECT：保留的⽅法名，⽤于代理切换隧道

其中，GET、POST、PUT、DELETE 四种方法被称为 RESTful API 的基本方法，用于表示对资源的不同操作。

### 头域

HTTP 头域是 HTTP 请求和响应消息中的一个重要组成部分，包含了关于消息的元数据信息。常见的 HTTP 头域信息有：

1. Accept：浏览器可接受的 MIME 类型
2. Accept-Encoding：浏览器可接受的编码方式
3. Accept-Language：浏览器可接受的语言
4. Cache-Control：控制缓存的行为
5. Connection：控制连接的行为
6. Content-Encoding：内容编码方式
7. Content-Length：内容长度
8. Content-Type：内容类型
9. Cookie：客户端保存的 Cookie 信息
10. Host：请求的主机名和端口号
11. Referer：请求的源 URL
12. User-Agent：客户端浏览器和操作系统的信息

除了上述常见的头域信息之外，HTTP 头域还包含了一些自定义的字段，用于在 HTTP 请求和响应消息中传递额外的元数据信息。通过自定义的 HTTP 头域信息，可以扩展 HTTP 协议的功能，为 HTTP 应用提供更多的信息和控制能力。

### 状态码

HTTP 状态码是 HTTP 协议中的一种标准化的响应状态代码，用于表示 HTTP 请求的处理结果。常见的 HTTP 状态码有：

1xx：信息性状态码，表示请求已被接收，继续处理。

- 100：继续。
- 101：服务器即将切换协议。

2xx：成功状态码，表示请求已成功被服务器接收、理解、并处理。

- 200：请求成功。
- 201：已创建。
- 202：已接受。
- 204：无内容。
- 206：部分内容。

3xx：重定向状态码，表示需要客户端采取进一步的操作才能完成请求。

- 301：永久重定向。
- 302：临时重定向。
- 303：查看其他位置。
- 304：未修改。
- 307：临时重定向，和 302 的区别在于不能更改 HTTP 的请求方法。

4xx：客户端错误状态码，表示客户端发送的请求有误，服务器无法处理该请求。

- 400：请求语法错误。
- 401：未授权。
- 403：禁止访问。
- 404：未找到。
- 405：方法禁用。
- 408：请求超时。

5xx：服务器错误状态码，表示服务器在处理请求的过程中发生了错误。

- 500：服务器内部错误。
- 501：尚未实施。
- 502：错误网关。
- 503：服务不可用。
- 504：网关超时。

HTTP 状态码的作用是让客户端能够了解请求的处理结果，以便客户端能够做出相应的处理。例如，当服务器返回 404 状态码时，客户端就知道请求的资源不存在，从而可以采取相应的处理措施。

### 跨域的解决方法

跨域指的是在 Web 开发中，一个域下的文档或脚本试图去请求另一个域下的资源，因为出于安全考虑，Web 浏览器的同源策略禁止了跨域资源的访问。为了解决这个问题，常用的跨域解决方案包括：

1. JSONP（JSON with Padding）：通过动态创建 script 标签，将需要获取的数据作为参数传递到指定的服务器端的 URL 上，服务器端将数据以函数调用的方式返回，并将函数调用的参数作为 JSON 格式数据返回，客户端通过回调函数处理返回的数据。
2. CORS（Cross-Origin Resource Sharing）：一种基于 HTTP 头的机制，它允许服务器指定哪些源可以访问资源，同时允许浏览器提交跨域请求。CORS 需要浏览器和服务器同时支持。
3. 代理：通过在同域下搭建一个代理服务器，让代理服务器去访问跨域的资源，然后再将请求结果返回给客户端，客户端只需要访问同域下的代理服务器即可，不再直接访问跨域的资源。
4. WebSocket：通过 WebSocket 协议在客户端和服务端之间建立持久连接，实现实时通信和数据传输。WebSocket 协议定义了客户端和服务端可以互相发送数据的一种新的网络通信协议。
5. postMessage：通过 window. postMessage 方法实现跨域通信，这个方法允许在两个窗口之间传递消息，不论这两个窗口是否同源。

### HTTP 缓存

HTTP 缓存是一种通过存储 HTTP 响应的副本来提高 Web 性能的机制。当 Web 浏览器请求一个资源（例如，一个图片、CSS 样式表或 JavaScript 文件）时，Web 服务器通常会将这些资源作为 HTTP 响应返回。**如果 HTTP 响应中包含缓存指令，浏览器就可以缓存响应，并在以后的请求中重用它们**。这意味着浏览器可以直接从缓存中获取资源，而不必再次从服务器请求，从而加快页面加载速度并减轻服务器的负担。

HTTP 缓存有两种类型：强制缓存和协商缓存。

**强制缓存**是指通过在响应头中设置**Expires 或 Cache-Control 指令**来指定缓存的时间，即在特定时间段内可以直接从缓存中获取响应，而不必再向服务器发起请求。

**协商缓存**是指通过在响应头中设置 Last-Modified 和 ETag 指令来指定响应内容的唯一标识符，并在下一次请求时使用 If-Modified-Since 和 If-None-Match 指令将这些标识符发送回服务器，以判断是否可以使用缓存的响应。如果响应未发生变化，则服务器将返回 304 Not Modified 状态码，并告诉浏览器可以使用缓存的响应。

HTTP 缓存是 Web 性能优化的重要方面，可以减少页面加载时间和网络带宽的消耗，并降低服务器的负载。

### 影响 HTTP 事务的 TCP 时延

HTTP 是基于 TCP 协议的，因此 TCP 的时延会影响 HTTP 事务的性能和响应时间。以下是几种可能影响 TCP 时延的因素：

1. 建立连接时延：建立 TCP 连接需要完成三次握手的过程，而每次握手都需要来回传递数据包，因此建立连接时延可能很高。
2. 传输时延：传输时延包括数据包从客户端发送到服务器的时间，以及从服务器发送响应到客户端的时间。这个时延受到数据包大小、网络带宽、网络拥塞等因素的影响。
3. 重传时延：如果一个数据包在传输过程中出现了错误或丢失，TCP 协议会触发重传机制，重新发送数据包。这个过程会导致额外的时延。
4. 拥塞时延：当网络中的流量过高时，会出现网络拥塞，这会导致 TCP 协议减缓传输速度，增加时延。
5. 关闭连接时延：关闭 TCP 连接需要完成四次挥手的过程，同样需要来回传递数据包，因此关闭连接时延也可能很高。

## 性能优化

### 影响性能的因素

1. 网络**延迟**：网络延迟会影响页面的加载速度，包括页面中的文本、图片、视频等内容。网络延迟受到很多因素的影响，如距离、网络拥塞等。
2. **带宽**限制：带宽限制会影响页面的下载速度，尤其是对于大型文件的下载，如视频、音频等。
3. 服务器响应时间：服务器响应时间包括服务端处理请求和返回响应的时间，影响用户等待页面加载完成的时间。
4. 页面和图片大小
5. **JavaScript 和 CSS**：JavaScript 脚本的执行和 CSS 文件的数量和大小也会影响页面性能，可以通过避免不必要的 DOM 操作、异步加载、合并 CSS 文件、压缩 CSS 代码等方式来优化。
6. **缓存**：缓存可以减少页面的请求次数，从而提高页面的加载速度。可以使用浏览器缓存、CDN 缓存等方式来优化。
7. 设备性能：设备性能也会影响页面的加载速度，较低的设备可能无法流畅地显示页面内容。
8. 传输协议 (TCP) 对 HTTP 的限制
9. HTTP 协议⾃身的功能和缺陷

### 性能指标

1. 页面加载时间（Page Load Time）：从用户请求页面到页面完全加载并可交互所需要的时间。通常使用页面加载完成的时间点作为页面加载时间。
2. 首次内容渲染时间（First Contentful Paint）：从用户请求页面到页面上第一块可见内容开始渲染的时间。这个指标反映了用户能够看到页面内容的速度。
3. 首次可交互时间（First Interactive Time）：从用户请求页面到页面变得可交互的时间。这个指标反映了用户可以开始与页面进行交互的时间。
4. 页面响应时间（Page Response Time）：服务器接收请求到响应完整返回所需要的时间。这个指标反映了服务器响应请求的速度。

### 浏览器渲染机制

1. HTML 解析：浏览器通过解析 HTML 文档来构建 DOM 树，将文档中的标签转化为 DOM 节点，并将它们组成一个树状结构，用于表示文档的层次结构。
2. CSS 解析：浏览器通过解析 CSS 样式文件来构建 CSSOM 树，将文档中的样式信息转化为 CSS 规则，并将它们组成一个树状结构，用于表示文档的样式信息。
3. 合成渲染树：浏览器将 DOM 树和 CSSOM 树合并成一棵渲染树（Render Tree），该树包含了所有需要显示的节点和它们的样式信息。
4. 布局：浏览器通过计算每个节点在屏幕上的位置和大小来确定渲染树中每个节点的准确位置。这个过程叫做布局（Layout）或重排（Reflow）。
5. 绘制：浏览器将渲染树中的节点绘制到屏幕上，生成最终的图像。这个过程叫做绘制（Paint）或重绘（Repaint）。

### 性能分析和优化方法

- 页面加载时间：常用的工具有 Google PageSpeed Insights 和 WebPageTest 等。
- 首次内容渲染时间：常用的工具有 WebPageTest 和 Lighthouse 等。
- CDN 加速：通过**CDN（内容分发网络）可以提高页面加载速度**，尤其是在跨国访问时。常用的 CDN 提供商有阿里云 CDN、腾讯云 CDN 等。
- 前端优化：优化前端代码可以提高页面的响应时间和渲染速度，例如**压缩 JavaScript 和 CSS 文件、缓存静态文件**等。
- 服务器优化：优化服务器配置和性能可以缩短页面响应时间，例如使用**高性能的 Web 服务器和数据库、优化代码和查询语句**等。
- 浏览器缓存：使用**浏览器缓存**可以减少页面的加载时间，但需要注意缓存的过期时间和更新策略。
- 前端性能监控工具：通过前端性能监控工具可以实时监控页面的性能指标，并进行性能优化。常用的工具有 Google Analytics、New Relic 等。

### 最佳实践

- 减少 DNS 查找：每⼀次主机名解析都需要⼀次⽹络往返, 从⽽增加请求的延迟时间, 同时还会阻塞后续请求。 
- 重⽤TCP 连接：尽可能使⽤持久连接, 以消除 TCP 握⼿和慢启动延迟。 
- 减少 HTTP 重定向：HTTP 重定向极费时间, 特别是不同域名之间的重定向, 更加费时; 这⾥⾯既有额外的 DNS 查询、TCP 握⼿, 还有其他延迟。最佳的重定向次数为零。 
- 使⽤CDN (内容分发⽹络)：把数据放到离⽤户地理位置更近的地⽅, 可以显著减少每次 TCP 连接的⽹络延迟, 增⼤吞吐量。这⼀条既适⽤于静态内容, 也适⽤于动态内容。 
- 去掉不必要的资源：任何请求都不如没有请求快。
- 在客户端缓存资源：应该缓存应⽤资源, 从⽽避免每次请求都发送相同的内容。
- 传输压缩过的内容：传输前应该压缩应⽤资源, 把要传输的字节减⾄最少，确保对每种要传输的资源，采⽤最好的压缩⼿段。
- 消除不必要的请求开销：减少请求的 HTTP ⾸部数据 (⽐如 HTTP cookie), 节省的时间相当于⼏次往返的延迟时间。
- 并⾏处理请求和响应：请求和响应的排队都会导致延迟, ⽆论是客户端还是服务器端。这⼀点经常被忽视, 但却会⽆谓地导致很⻓延迟。 
- 针对协议版本采取优化措施：HTTP 1. x ⽀持有限的并⾏机制, 要求打包资源、跨域分散资源, 等等。相对⽽⾔, HTTP 2.0 只要建⽴⼀个连接就能实现最优性能, 同时⽆需针对 HTTP 1. x 的那些优化⽅法（例如域名分区、文件拼接和图片精灵），而应该多使用多路复用、服务器推送等功能。

## 前端鉴权

### 术语

**认证**是指验证用户身份是否正确，常见的认证方式包括用户名密码认证、OAuth 认证等。

**授权**是指根据用户身份的不同，授予不同的权限。在互联网领域： **web 服务器的 session 机制、web 浏览器的 cookie 机制、授权令牌（token）机制**。

**鉴权**是指对用户所声明的真实性进⾏鉴别确认的过程，是⼀个承上启下的⼀个环节，上游它接受授权的输出，校验其真实性后，然后获取权限，为下⼀步的权限控制做好准备。在互联网领域：**校验 session/cookie/token 的合法性和有效性**

**权限控制** (Access/Permission Control) 将可执⾏的操作定义为权限列表，然后判断操作是否允许/禁⽌。在互联网领域：**通过 web 后端服务，来控制接⼝访问，允许或拒绝访问请求**。

认证、授权、鉴权和权限控制这四个环节是⼀个前后依次发⽣、上下游的关系

![前端鉴权 1.png](https://cdn.hcplantern.cn/img/2023/02/22/20230222-210957.awebp%3F-default)

**SSO (Single Sign On) 单点登录**：在**同域下的客户端/服务端认证系统**中，通过客户端携带凭证，可以维持⼀段时间内的登录状态。比如在一个大型系统中存在许多子系统，只需要登录⼀次，就可以访问其他相互信任的应⽤系统。

**OAuth 2.0**：OAuth 是⼀个开放标准，允许⽤户授权第三⽅⽹站 (CSDN、思否等) 访问他们存储在另外的服务提供者上的信息，⽽不需要将⽤户名和密码提供给第三⽅⽹站；

### 鉴权方案

简单总结：

- HTTP 基本鉴权就是通过弹出用户名密码输入框，向服务器传输 Base64 用户名密码
- Session-Cookie 是在服务器存一个 Session ID，对应一个 Cookie 存在客户端
- Token 是服务器生成令牌给用户，包含用户 uid，所以**需要查询数据库**获取用户的其他信息
- JWT 是将用户信息组成 JSON 对象再进行加密形成 Token，充分**利用 token 本身所携带**的信息

#### HTTP 基本鉴权

客户端（通常指的就是⽹⻚浏览器）在请求时，通过⽤户提供⽤户名和密码的⽅式，实现对⽤户身份的验证。

![1.HTTP基本鉴权.png](https://cdn.hcplantern.cn/img/2023/02/22/20230222-211042.awebp%3F-default)

优点：

- 简单，基本所有流⾏的浏览器都⽀持

缺点：

- 不安全：基于 HTTP 传输，所以它在⽹络上⼏乎是裸奔的；
- 无法主动注销：HTTP 协议没有提供机制清除浏览器中的 Basic 认证信息

#### Session-Cookie 鉴权

Session-Cookie 鉴权是一种常见的用户身份认证方式。它的基本原理是，用户在登录系统后，**服务器为该用户创建一个 Session 对象**，同时生成一个唯一的 Session ID，并**将该 Session ID 以 Cookie 的形式发送给客户端浏览器**。当用户访问系统的其他页面时，浏览器会自动携带该 Cookie，服务器通过该 Cookie 获取 Session ID，然后验证该 Session ID 是否有效。如果有效，说明用户已经登录，可以访问该页面；如果无效，则要求用户重新登录。

![2.Session-Cookie的认证流程图.png](https://cdn.hcplantern.cn/img/2023/02/22/20230222-211013.awebp%3F-default)

Session 与 Cookie 对比：

- 安全性：Session 保存在服务器上，通过服务器自带的加密协议进行，安全性更高。Cookie 保存在客户端，可以随意篡改。
- 存取值的类型： Cookie 只⽀持字符串数据，Session 可以存任意数据类型；
- 有效期不同： Cookie 可设置为⻓时间保持，Session ⼀般失效时间较短；
- 存储⼤⼩不同： Cookie 保存的数据不能超过 4K；

优点：

- Cookie 简单易用
- Session 数据存储在服务端，方便管理。相较于 JWT 方便进行管理，也就是当用户登录和主动注销，只需要添加删除对应的 Session 就可以了
- 只需要后端操作即可，前端可以无感等进行操作

缺点：

- **依赖 Cookie**，一旦用户在浏览器端禁用 Cookie，就无法使用
- 非常**不安全**，Cookie 将数据暴露在浏览器中，增加了数据被盗的风险（容易被 CSRF 等攻击）
- Session 存储在服务端，**增大了服务端的开销**，用户量大的时候会大大降低服务器性能
- **对移动端的支持性不友好**

#### Token （令牌）

`Token` 是一个令牌，客户端访问服务器时，验证通过后服务端会为其签发一张令牌，之后，客户端就可以携带令牌访问服务器，服务端只需要验证令牌的有效性即可。

一句话概括；**访问资源接口（API）时所需要的资源凭证**

**一般 Token 的组成：** **uid** (用户唯一的身份标识) + **time** (当前时间的时间戳) + **sign** (签名，Token 的前几位以哈希算法压缩成的一定长度的十六进制字符串)

![3.Token的认证流程图.png](https://cdn.hcplantern.cn/img/2023/02/22/20230222-211454.awebp%3F-default)

**Token 的优点：**

- **服务端无状态化、可扩展性好：** Token 机制在服务端**不需要存储会话（Session）信息**，因为 Token 自身包含了其所标识用户的相关信息，这有利于在多个服务间共享用户状态
- **支持 APP 移动端设备；**
- **安全性好：** 不需要 Cookie，有效避免 CSRF 攻击
- **支持跨程序调用：** 因为 Cookie 是不允许跨域访问的，而 Token 则不存在这个问题

**Token 的缺点：**

- **配合**：需要前后端配合处理；
- **占带宽**：正常情况下比 `sid` 更大，消耗更多流量，挤占更多宽带
- **性能问题**：需要对 Token 加解密以及查询数据库等操作，更耗性能；
- **有效期短**：为了避免 Token 被盗用，一般 Token 的有效期会设置的较短，所以就有了 `Refresh Token`；

#### Refresh Token（刷新 Token）：

为了安全，我们的 `Access Token` 有效期一般设置较短，以避免被盗用。但过短的有效期会造成 `Access Token` 经常过期，过期后怎么办呢？

一种办法是：`刷新 Access Token`，让用户重新登录获取新 Token，会很麻烦；

另外一种办法是：再来一个 Token，一个**专门生成 Access Token 的 Token**，我们称为 `Refresh Token`；

- **Access Token：** 用来访问业务接口，由于有效期足够短，盗用风险小，也可以使请求方式更宽松灵活；
- **Refresh Token：** 用来获取 Access Token，有效期可以长一些，通过独立服务和严格的请求方式增加安全性；由于不常验证，也可以如前面的 Session 一样处理；

![3.1RefreshToken的认证流程图.png](https://cdn.hcplantern.cn/img/2023/02/22/20230222-212148.awebp%3F-default)

#### Token 和 Session-Cookie 的区别

- **存储地不同：** Session 一般是存储在服务端；Token 是无状态的，一般由前端存储；
- **安全性不同：** Session 和 Token 并不矛盾，作为身份认证 Token 安全性比 Session 好，因为每一个请求都有签名还能防止监听以及重放攻击；
- **支持性不同：** Session-Cookie 认证需要靠浏览器的 Cookie 机制实现，如果遇到原生 NativeAPP 时这种机制就不起作用了，或是浏览器的 Cookie 存储功能被禁用，也是无法使用该认证机制实现鉴权的；而 Token 验证机制丰富了客户端类型。

#### JWT （JSON Web Token）

Token 鉴权的问题在于服务端需要查询数据库从而验证 Token 是否有效，增加性能消耗。

`JWT` 是 `Auth0` 提出的通过 `对 JSON 进行加密签名`来实现授权验证的方案；

就是登录成功后将**相关用户信息组成 JSON 对象**，然后对这个对象进行某种方式的`加密`，返回给客户端；客户端在下次请求时带上这个 Token；服务端再收到请求时`校验 token 合法性`，其实也就是在校验请求的合法性。

JWT 由三部分组成： Header 头部、 Payload 负载和 Signature 签名

客户端收到服务器返回的 JWT，可以储存在 Cookie 里面，也可以储存在 localStorage。

此后，客户端每次与服务器通信，都要带上这个 JWT。你可以把它放在 Cookie 里面自动发送，但是这样不能跨域，所以更好的做法是放在 HTTP 请求的头信息 `Authorization` 字段里面。

其实 JWT 的认证流程与 Token 的认证流程差不多，只是**不需要再单独去查询数据库查找用户**：

![4.JWT的认证流程图的副本.png](https://cdn.hcplantern.cn/img/2023/02/22/20230222-213017.awebp%3F-default)

JWT 的优点

- 不需要在服务端保存会话信息（RESTful API 的原则之一就是**无状态**），所以易于应用的扩展，即信息不保存在服务端，不会存在 Session 扩展不方便的情况；
- JWT 中的 **Payload 负载可以存储常用信息**，用于信息交换，有效地使用 JWT，可以降低服务端查询数据库的次数

JWT 的缺点

- **加密问题：** JWT 默认是不加密，但也是可以加密的。生成原始 Token 以后，可以用密钥再加密一次。
- **到期问题：** 由于服务器不保存 Session 状态，因此无法在使用过程中废止某个 Token，或者更改 Token 的权限。也就是说，一旦 JWT 签发了，在到期之前就会始终有效，除非服务器部署额外的逻辑。

### 其他方案

单点登录（Single Sign On）

OAuth 2.0

联合登录和信任登录

唯⼀登录

扫码登录

⼀键登录（适⽤于原⽣APP）

## 登陆注册

- 选择什么登陆和注册方式：手机号码、电子邮箱、用户名、其他社交帐号、二维码
- 密码加密传输方案：
  - ![image-20230222215607964](https://cdn.hcplantern.cn/img/2023/02/22/20230222-215609.png-default)
  - 普通的 Hash 函数：MD5, SHA-1, SHA-2, SHA-3 等
  - 应对普通哈希容易被破解的策略：加盐，慢哈希
  - 使用密码哈希函数：PBKDF2，Bcrypt，Scrypt，Argon2
- 注册和登陆界面的元素细节（密码重复、用户协议）
- 报错提示设计
- 密码强度
- CAPTCHA 验证码

## 前端主题切换

### link 标签动态引入

其做法就是提前准备好几套 `CSS` 主题样式文件，在需要的时候，创建 `link` 标签动态加载到 `head` 标签中，或者是动态改变 `link` 标签的 `href` 属性。

![图片](https://cdn.hcplantern.cn/img/2023/02/22/20230222-214717-default)

表现效果如下：

![图片](https://cdn.hcplantern.cn/img/2023/02/22/20230222-214716-default)

网络请求如下：

![图片](https://cdn.hcplantern.cn/img/2023/02/22/20230222-214714-default)

优点：

- 实现了按需加载，提高了首屏加载时的性能

缺点：

- 动态加载样式文件，如果文件过大网络情况不佳的情况下可能会有加载延迟，导致**样式切换不流畅**
- 如果主题样式表内定义不当，会有优先级问题
- 各个主题样式是写死的，后续针对某一主题样式表修改或者新增主题也很麻烦

### 类名切换

这种方案与第一种比较类似，为了解决反复加载样式文件问题提前将样式全部引入，在需要切换主题的时候**将指定的根元素类名更换**，相当于直接做了样式覆盖，在该类名下的各个样式就统一地更换了。其基本方法如下：

```css
/* day样式主题 */
body.day .box {
  color: #f90;
  background: #fff;
}
/* dark样式主题 */
body.dark .box {
  color: #eee;
  background: #333;
}

.box {
  width: 100px;
  height: 100px;
  border: 1px solid #000;
}
<div class="box">
  <p>hello</p>
</div>
<p>
  选择样式：
  <button onclick="change('day')">day</button>
  <button onclick="change('dark')">dark</button>
</p>
function change(theme) {
  document.body.className = theme;
}
```

表现效果如下：

![图片](https://cdn.hcplantern.cn/img/2023/02/22/20230222-214555-default)

优点：

- 不用重新加载样式文件，在样式切换时不会有卡顿

缺点：

- 首屏加载时会牺牲一些时间加载样式资源
- 如果主题样式表内定义不当，也会有优先级问题
- 扩展性不佳，各个主题样式是写死的，后续针对某一主题样式表修改或者新增主题也很麻烦

### CSS 变量+类名切换

大体思路跟方案 2 相似，依然是提前将样式文件载入，切换时将指定的根元素类名更换。不过这里相对灵活的是，**默认在根作用域下定义好 CSS 变量，只需要在不同的主题下更改 CSS 变量对应的取值即可。**

```css
/* 定义根作用域下的变量 */
:root {
  --theme-color: #333;
  --theme-background: #eee;
}
/* 更改dark类名下变量的取值 */
.dark{
  --theme-color: #eee;
  --theme-background: #333;
}
/* 更改pink类名下变量的取值 */
.pink{
  --theme-color: #fff;
  --theme-background: pink;
}

.box {
  transition: all .2s;
  width: 100px;
  height: 100px;
  border: 1px solid #000;
  /* 使用变量 */
  color: var(--theme-color);
  background: var(--theme-background);
}
```

优点：

- 不用重新加载样式文件，在样式切换时不会有卡顿
- 在需要切换主题的地方利用 var () 绑定变量即可，不存在优先级问题
- 新增或修改主题方便灵活，仅需新增或修改 CSS 变量即可，在 var () 绑定样式变量的地方就会自动更换

缺点：

- IE 兼容性（忽略不计）
- 首屏加载时会牺牲一些时间加载样式资源
