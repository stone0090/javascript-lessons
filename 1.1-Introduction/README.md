# 简介

JavaScript 是面向 Web 的编程语言，绝大多数现代网站都使用了 JavaScript，并且所有的现代 Web 浏览器（电脑，手机，平板）均包含了 JavaScript 解释器。 这使得 JavaScript 能够称得上史上使用最广泛的编程语言。 JavaScript 也是前端开发工程师必须掌握的三种技能之一：描述网页内容的 HTML、描述网页样式的 CSS、以及描述网页行为的 JavaScript。

JavaScript 是一门 **动态的、弱类型的、面向对象的、解释型的** 编程语言，非常适合面向对象和函数式的编程风格。JavaScript 的语法来自于 Java，它的一等函数（first-class function）来自于 Scheme，它的基于原型（prototype-based）的继承来自于 Self。但学习本课程不必去了解那些（Java/Scheme/Slef）语言或熟悉那些术语。

## JavaScript 起源

> 时间：1995年

> 人物：Brendan Eich（布兰登·艾奇）

> 背景：那个时候，绝大多数因特网用户都是用速度仅为28.8kbit/s的「猫」上网，为了完成简单的表单验证，必须把表单数据发送到服务器端才能确定用户是否没有填写某个必填域、是否输入了无效的值，每次操作需等待30秒以上才会有结果，这无疑是在慢性自杀。

> 事件：当时走在技术革新最前沿的 Netscape 公司指派 Brendan Eich 开发一种客户端语言，用来处理这种简单的验证，它就是 JavaScript。JavaScript 原名 LiveScript，Netscape 为了搭上媒体热炒 Java 的顺风车，才把 LiveScript 改名为 JavaScript。（所以 Java 和 JavaScript 的关系，就相当于雷锋和雷峰塔的关系。）

自此以后，JavaScript 逐渐成为市面上常见浏览器必备的一项特色功能。如今，JavaScript 的用途早已不再局限于简单的数据验证，而是具备了与浏览器窗口及其内容等几乎所有方面交互的能力。今天的 JavaScript 已经成为一门功能全面的编程语言，能够处理复杂的计算和交互，拥有了闭包、匿名（lambda，拉姆达）函数，甚至元编程等特性。

JavaScript 从一个简单的输入验证器发展成为一门强大的编程语言，完全出乎人们的意料。应该说，它既是一门非常简单的语言，又是一门非常复杂的语言。说它简单，是因为学会使用它只需片刻功夫；而说它复杂，是因为要真正掌握它则需要数年时间。

## JavaScript 实现

随着 JavaScript 的普及，微软公司在 IE 浏览器中也加入了名为 JScript 的 JavaScript 实现，由于没有标准规定 JavaScript 的语法和特性，导致市面上存在多个版本的 JavaScript 实现，且互不兼容，因此 JavaScript 标准化问题被提上了议事日程。

1997年，欧洲计算机制造商协会（ECMA，European Computer Manufactures Association）定义了名为ECMAScript（发音为 `ek-ma-script`）的脚本语言标准，它是通用的，与平台无关的语言标准。自此以后，浏览器开发商开始致力于将 ECMAScript 作为各自 JavaScript 实现的基础。虽然基础相同，但具体实现在不同浏览器上却略有差异。

JavaScript 和 ECMAScript 通常被人们用来表达相同的含义，但 JavaScript 的含义却比 ECMAScript 中规定的要多得多。一个完整的 JavaScript 实现应该由下面三个不同的部分组成。

- 核心（ECMAScript）
- 文档对象模型（DOM）
- 浏览器对象模型（BOM）

### 核心（ECMAScript）

ECMAScript 标准由 **语法、数据类型、语句、关键字、保留字、运算符、对象** 组成。它与 Web 浏览器没有任何依赖关系，并且这门语言本身并不包含输入和输出定义。ECMAScript 定义的只是这门语言的基础，而在此基础之上可以构建更完善的脚本语言。

Web 浏览器只是 ECMAScript 实现的宿主环境之一，其他宿主环境包括 Node 和 Adobe Flash。宿主环境不仅提供基本的 ECMAScript 实现，同时也会提供该语言的扩展（例如：DOM、BOM），这些扩展则利用 ECMAScript 的核心类型和语法提供更多更具体的功能。

**JavaScript 是 Web 浏览器对 ECMAScript 标准的实现**，ActionScript 是 Adobe Flash 对 ECMAScript 标准的实现。

#### ECMAScript 历史

> 1997年，ECMAScript 1版发布。

> 1998年6月，ECMAScript 2版发布。

> 1999年12月，ECMAScript 3版发布。

> 2000年，ECMAScript 4开始酝酿，最终这个版本没有通过。

> 2009年12月，ECMAScript 5版发布。

> 2011年6月，ECMAscript 5.1版发布，成为国际标准。

> 2015年6月，ECMAScript 6正式通过，成为国际标准。

> 扩展阅读「[阮一峰](http://www.ruanyifeng.com/home.html) 的《ECMAScript 6入门》」  
> http://es6.ruanyifeng.com/#docs/intro

3.0版是一个巨大的成功，在业界得到广泛支持，成为通行标准，奠定了 JavaScript 语言的基本语法，以后的版本完全继承。直到今天，初学者一开始学习 JavaScript，其实就是在学3.0版的语法。

### 文档对象模型（DOM）

文档对象模型（DOM，Document Object Model）是用于 HTML 的应用程序编程接口（API），它把整个页面映射为一个多层节点结构。HTML 页面中的每个组成部分都是某种类型的节点，这些节点又包含着不同类型的数据。看下面这个 HTML 页面：

``` html
<html>
    <head>
        <title>Sample Page</title>
    </head>
    <body>
        <p>Hello World!</p>
    </body>
</html>
```

通过 DOM 创建的这个表示文档的树形图，开发人员获得了控制页面内容和结构的主动权。借助 DOM 提供的 API，开发人员可以轻松自如地删除、添加、替换或修改任何节点。

由于 Netscape 和 微软实现的 DOM 互不兼容，负责制定 Web 通信标准的 W3C（World Wide Web Consortium，万维网联盟）开始着手规划 DOM。

#### DOM 1级：
- DOM 核心：映射文档结构，简化对文档中任意部分的操作和访问。
- DOM HTML：在 DOM 核心的基础上，添加了针对 HTML 的对象和方法。

#### DOM 2级：
- DOM 视图：定义了跟踪不同文档视图的接口。
- DOM 事件：定义了事件和事件处理的接口。
- DOM 样式：定义了基于 CSS 为元素应用样式的接口。
- DOM 遍历和范围：定义了遍历和操作文档树的接口。

#### DOM 3级：
- DOM 加载和保存：引入了以统一方式加载和保存文档的方法。
- DOM 验证：新增了验证文档的方法。
- DOM 核心扩展。

> 注意：  
> 1. DOM 并不是只针对 JavaScript 的，很多别的语言也都实现了 DOM。  
> 2. DOM 0级标准是不存在的，它只是 DOM 历史坐标中的一个参照点而已。

### 浏览器对象模型（BOM）

浏览器对象模型（BOM，Browser Object Model）是用于浏览器的应用程序编程接口（API），它把整个浏览器窗口映射为一个对象。从根本上讲，BOM 只处理浏览器窗口和框架，但人们习惯上也把所有针对浏览器的 JavaScript 扩展算作 BOM 的一部分，例如：

- 弹出新浏览器窗口的功能。
- 移动、缩放和关闭浏览器窗口的功能。
- 提供浏览器详细信息的 navigator 对象。
- 提供浏览器所加载页面的详细信息的 localtion 对象。
- 提供用户显示器分辨率详细信息的 screen 对象。
- 对 cookies 的支持。
- XMLHttpRequest 和 IE 的 ActiveXObject 这样的自定义对象。

BOM 最让人头疼的是没有相关的规范和标准，每个浏览器都有独有的实现，这个问题在 HTML5 中得到了解决，HTML5 致力于把很多 BOM 功能写入正式规范。


## 小结
JavaScript 是一种专为网页交互而设计的脚本语言，由下列3个不同的部分组成：

- 核心（ECMAScript），由 ECMA-262 定义，提供核心语言功能。
- 文档对象模型（DOM），提供访问和操作网页内容的方法和接口。
- 浏览器对象模型（BOM），提供与浏览器交互的方法和接口。

JavaScript 的这3个组成部分，在当前5个主要浏览器（IE、FireFox、Chrome、Safari 和 Opera）中都得到了不同程度的支持。其中，所有浏览器对 ECMAScript 3版本的支持大体上都还不错，而对 ECMAScript 5的支持程度越来越高，但对 DOM 的支持则彼此相差比较多。对于已经正式纳入 HTML5 标准的 BOM 来说，尽管各浏览器都实现了某些众所周知的共同特性，但其他特性还是会因浏览器而异。

> 扩展阅读「Standard ECMA-262 5.1 Edition 」  
> http://www.ecma-international.org/ecma-262/5.1/

> 扩展阅读「ECMAScript 5浏览器兼容一览表」  
> http://caniuse.mojijs.com/Home/Html/item/key/es5/index.html

## 更多

> 关注微信公众号「劼哥舍」回复「答案」，获取关卡详解。  
> 关注 [https://github.com/stone0090/javascript-lessons](https://github.com/stone0090/javascript-lessons)，获取最新动态。

