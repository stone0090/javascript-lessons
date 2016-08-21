# 语法

JavaScript 的语法大量借鉴了 C 及其他类 C 语言（如 Java 和 Perl）的语法。因此，熟悉这些语言的开发人员在接受 JavaScript 更加宽松的语法时，一定会有种轻松自在的感觉。本章主要按照 ECMAScript 3介绍这门语言的基本概念，并就 ECMAScript 5的变化给出说明。

## 字符集

JavaScript 程序是用 Unicode 字符集编写的，Unicode 是 ASCII 和 Latin-1 的超集，并支持地球上几乎所有在用的语言。ECMAScript 3要求 JavaScript 的实现必须支持 Unicode 2.1及后续版本，ECMAScript 5则要求支持 Unicode 3及后续版本。

> 扩展阅读「Unicode 与 JavaScript 详解」  
> http://www.ruanyifeng.com/blog/2014/12/unicode.html

## 区分大小写

JavaScript 是区分大小写的。也就是说，关键字、变量、函数名和所有的标识符（identifier）都必须采取一致的大小写形式。比如，关键字 `while` 必须写成 `while`，而不能写成 `While` 或者 `WHILE`。同样，`online`、`Online`、`OnLine`、`ONLINE` 是4个不同的变量名。

但需要注意的是，HTML 并不区分大小写。由于它和客户端 JavaScript 联系紧密，因此这点区别很容易混淆。许多客户端 JavaScript 对象和属性与他们所表示的 HTML 标签和属性名相同。在 HTML 中，这些标签和属性名可以使用大写也可以是小写，而在 JavaScript 中则必须是小写。例如，在 HTML 中设置事件处理程序时，`onclick` 属性可以写成 `onClick`，但在 JavaScript 代码中，必须使用小写的 `onclick`。

## 注释

JavaScript 使用 C 风格的注释，包括单行注释和块级注释。单行注释以两个斜杠 `//` 开头，块级注释以一个斜杠和一个星号 `/*` 开头，以一个星号和一个斜杠 `*/` 结尾。下面都是合法的 JavaScript 注释：

``` javascript
// 这里是单行注释

/*
 *  这里是块级注释
 *  也叫多行注释
 */
```

虽然上面注释中的第二和第三行都以一个星号开头，但这不是必须的，纯粹是为了提高注释的可读性（这种格式在企业级应用中极其常见）。

## 字面量

所谓字面量（也可称直接量，Literal values），就是程序中直接使用的数据值。字面量只代表自身，不存储在特定位置。JavaScript 中的字面量有：字符串、数字、布尔值、对象、数组、函数、正则表达式，以及特殊的 `null` 值。

``` javascript
"hello world"   // 字符串
123             // 数字
1.2             // 小数
true            // 布尔值
false           // 布尔值
/javascript/gi  // 正则表达式
null            // 空
{ name: 'stone', age: 20}       // 对象
[ 1, 2, 3, 4, 5, 6, 7, 8 ]      // 数组
function(){ console.log('good'); }    // 函数
```

> 扩展阅读「undefined不是字面量」  
> http://www.cnblogs.com/ziyunfei/archive/2012/11/11/2765096.html

## 严格模式

在 ECMAScript 5引入了严格模式（strict mode）的概念。严格模式是为 JavaScript 定义了一种不同的解析与执行模式。在严格模式下，ECMAScript 3中的一些不确定的行为将得到处理，而且对某些不安全的操作也会抛出错误。要在整个脚本中启用严格模式，可以在顶部添加如下代码：
 
 ``` javascript
 "use strict";
 ```

 这行代码看起来像是字符串，而且也没有赋值给任何变量，但其实它是一个编译指示（pragma），用于告诉支持的 JavaScript 引擎切换到严格模式。这是为了不破坏 ECMAScript 3语法而特意选定的语法。

 在函数内部的第一行包含这条编译指示，也可以指定函数在严格模式下执行：

``` javascript
function doSomething(){
    "use strict"; 
    // 函数体
}
```

严格模式下，JavaScript 的执行结果会有很大不同，因此本课程会随时指出严格模式下的区别。

## 标识符

所谓标识符，就是指变量、函数、属性的名字，或者函数的参数。JavaScript 标识符必须以字母、下划线（`_`）或美元符号（`$`）开始，后续的字符可以是字母、数字、下划线或美元符号（数字是不允许作为首字符出现的）。下面是合法的标识符：
``` javascript
my_variable_name, v12345, _dummy, $str888
```

标识符中的字母可以包含扩展的 ASCII 或 Unicode 字母字符（如 `π` 和 `∑`），但不推荐这样做。按照惯例，JavaScript 标识符采用驼峰大小写格式，也就是第一个字母小写，剩下的每个有意义的单词的首字母大写，例如：

``` javascript
firstSecond, myCar, doSomethingImportant
```

## 关键字和保留字

ECMAScript 3描述了一组具有特定用途的关键字，这些关键字可用于表示控制语句的开始或结束，或者用于执行特定操作等。按照规则，关键字是语言保留的，不能用作标识符。以下是 ECMAScript 3的全部关键字：

``` javascript
break       delete      function    return      typeof
case        do          if          switch      var
catch       else        in          this        void
continue    false       instanceof  throw       while
debugger    finally     new         true        with
default     for         null        try
```

ECMAScript 3还将 Java 的所有关键字都作为自己的保留字。尽管保留字还没有任何特定的用途，但他们有可能在将来被用作关键字：

``` javascript
abstract    double      goto        native      static
boolean     enum        implements  package     super
byte        export      import      private     synchronized
char        extends     int         protected   throws
class       final       interface   public      transient
const       float       long        short       volatile
```

ECMAScript 5把非严格模式下的保留字缩减为：

``` javascript
class           enum            extends         super
const           export          import
```

ECMAScript 5在严格模式下的保留字为：

``` javascript
implements      package         public
interface       private         static
let             protected       yield
```

注意，`let` 和 `yield` 是 ECMAScript 5新增的保留字，其他保留字都是 ECMAScript 3定义的。为了保证兼容性，任何时候都不建议使用 ECMAScript 5新增的保留字 `let` 和 `yield` 。

ECMAScript 还预定义了很多全局变量和函数，也应当避免把它们用作标识符：

``` javascript
arguments           Error           Math            RegExp
Array               eval            NaN             String
Boolean             EvalError       Number          SyntaxError
Date                Function        Object          TypeError
decodeURI           Infinity        parseFloat      undefined
decodeURIComponent  isFinite        parseInt        URIError
encodeURI           isNaN           RangeError
encodeURIComponent  JSON            ReferenceError
```

JavaScript 的具体实现可能定义独有的全局变量和函数，每一种特定的 JavaScript 运行环境都有自己的一个全局属性列表，这一点是需要牢记的。

## 可选的分号

和其他许多变成语言一样，JavaScript 使用分号（`;`）将语句分隔开。这对增强代码的可读性和整洁性是非常重要的。缺少分隔符，一条语句的结束就成了下一条语句的开始，反之亦然。如果语句各自独占一行，通常可以省略语句之间的分号（程序结尾或花括号 `}` 之前的分号也可以省略）。

``` javascript
var sum = a + b     // 即使没有分号也是有效的语句，不好的写法
var diff = a - b;   // 有效的语句，好的写法
```

虽然语句结尾的分号不是必须的，但请任何时候都不要省略它。因为加上这个分号可以避免很多错误，开发人员也可以放心地通过删除多余的空格来压缩 JavaScript 代码。另外，加上分号也会在某些情况下增进代码的性能，因为这样解析解就不必再花时间推测应该在哪插入分号了。

## 关卡

请判断以下代码是否有效？如果有效请给出结果，如果无效请说明原因。

``` javascript
// 挑战一
var class = 'person';
console.log(class);     // ???
```

``` javascript
// 挑战二
var Class = 'person';
console.log(class);     // ???
```

``` javascript
// 挑战三
var True = false;
console.log(True);      // ???
```

``` javascript
// 挑战四
var true = false;
console.log(True);      // ???
```

``` javascript
// 挑战五
var $_$ = 'stone';
console.log($_$);       // ???
```

``` javascript
// 挑战六
var 00_name = 'stone';
console.log(00_name);   // ???
```

``` javascript
// 挑战七
var Array = 'null';
console.log(Array);     // ???
```

``` javascript
// 挑战八
var undefined = 'null';
console.log(undefined); // ???
```

``` javascript
// 挑战九
var result = 1 + 2
+ 3 + 4
console.log(result);    // ???
```

## 更多

> 关注微信公众号「石佳劼的博客」回复「答案」，可获取关卡详解。  
> 您还可以在 [GitHub](https://github.com/) 上 [Star](https://github.com/stone0090/javascript-lessons) 该课程，查看章节目录，获取最新内容。  
> https://github.com/stone0090/javascript-lessons

![](http://7xkhp9.com1.z0.glb.clouddn.com/blog/other/blog_statement_20160618_01.png?imageView2/2/w/650/interlace/1/q/100)