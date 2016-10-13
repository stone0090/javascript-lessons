# 作用域和闭包

在编程语言中，作用域控制着变量与参数的可见性及生命周期。对程序员来说这是一项重要的服务，因为它减少了名称冲突，并且提供了自动内存管理。

## 作用域

一个变量的作用域（scope）是程序源代码中定义这个变量的区域。全局变量拥有全局作用域，在 JavaScript 代码中的任何地方都是有定义的。然而在函数内声明的变量只在函数体内有定义。它们是局部变量，作用域是局部性的。函数参数也是局部变量，它们只在函数体内有定义。

在函数体内，局部变量的优先级高于同名的全局变量。如果在函数内声明的一个局部变量或者函数参数中带有的变量和全局变量重名，那么全局变量就被局部变量所遮盖。

```javascript
var scope = "global";      // 声明一个全局变量
function checkscope() {
    var scope = "local";   // 声明一个同名的局部变量
    return scope;          // 返回局部变量的值，而不是全局变量的值
}
console.log(checkscope()); // "local"
```

尽管在全局作用域编写代码时可以不写 `var` 语句，但声明局部变量时则必须使用 `var` 语句。思考一下如果不这样做会怎样：

```javascript
scope = "global";           // 声明一个全局变量，甚至不用 var 来声明
function checkscope2() {
    scope = "local";        // 糟糕！我们刚修改了全局变量
    myscope = "local";      // 这里显式地声明了一个新的全局变量
    return [scope, myscope];// 返回两个值
}
console.log(checkscope2()); // ["local", "local"]，产生了副作用
console.log(scope);         // "local"，全局变量修改了
console.log(myscope);       // "local"，全局命名空间搞乱了
```

函数定义是可以嵌套的。由于每个函数都有它自己的作用域，因此会出现几个局部作用域嵌套的情况，例如：

```javascript
var scope = "global scope";         // 全局变量
function checkscope() {
    var scope = "local scope";      //局部变量 
    function nested() {
        var scope = "nested scope"; // 嵌套作用域内的局部变量
        return scope;               // 返回当前作用域内的值
    }
    return nested();
}
console.log(checkscope());          // "nested scope"
```

### 函数作用域和声明提前

在一些类似 C 语言的编程语言中，花括号内的每一段代码都具有各自的作用域，而且变量在声明它们的代码段之外是不可见的，我们称为块级作用域（block scope），而 JavaScript 中没有块级作用域。JavaScript 取而代之地使用了函数作用域（function scope），变量在声明它们的函数体以及这个函数体嵌套的任意函数体内都是有定义的。

在如下所示的代码中，在不同位置定义了变量 `i`、`j` 和 `k`，它们都在同一个作用域内，这三个变量在函数体内均是有定义的。

```javascript
function test(o) {
    var i = 0; // i在整个函数体内均是有定义的
    if (typeof o == "object") {
        var j = 0; // j在函数体内是有定义的，不仅仅是在这个代码段内
        for (var k = 0; k < 10; k++) { // k在函数体内是有定义的，不仅仅是在循环内
            console.log(k); // 输出数字0~9
        }
        console.log(k); // k已经定义了，输出10
    }
    console.log(j); // j已经定义了，但可能没有初始化
}
```

JavaScript 的函数作用域是指在函数内声明的所有变量在函数体内始终是可见的。有意思的是，这意味着变量在声明之前甚至已经可用。JavaScript 的这个特性被非正式地称为声明提前（hoisting），即 JavaScript 函数里声明的所有变量（但不涉及赋值）都被「提前」至函数体的顶部，看一下如下代码：

```javascript
var scope = "global";
function f() {
    console.log(scope);  // 输出"undefined"，而不是"global"
    var scope = "local"; // 变量在这里赋初始值，但变量本身在函数体内任何地方均是有定义的
    console.log(scope);  // 输出"local"
}
```

你可能会误以为函数中的第一行会输出 `"global"`，因为代码还没有执行到 `var` 语句声明局部变量的地方。其实不然，由于函数作用域的特性，局部变量在整个函数体始终是有定义的，也就是说，在函数体内局部变量遮盖了同名全局变量。尽管如此，只有在程序执行到 `var` 语句的时候，局部变量才会被真正赋值。因此，上述过程等价于：将函数内的变量声明“提前”至函数体顶部，同时变量初始化留在原来的位置：

```javascript
function f() {
    var scope;          // 在函数顶部声明了局部变量
    console.log(scope); // 变量存在，但其值是"undefined"
    scope = "local";    // 这里将其初始化并赋值
    console.log(scope); // 这里它具有了我们所期望的值
}
```

在具有块级作用域的编程语言中，在狭小的作用域里让变量声明和使用变量的代码尽可能靠近彼此，通常来讲，这是一个非常不错的编程习惯。由于 JavaScript 没有块级作用域，因此一些程序员特意将变量声明放在函数体顶部，而不是将声明靠近放在使用变量之处。这种做法使得他们的源代码非常清晰地反映了真实的变量作用域。

### 作用域链

当代码在一个环境中执行时，会创建变量对象的一个**作用域链**（scope chain）。作用域链的用途，是保证对执行环境有权访问的所有变量和函数的有序访问。作用域链的前端，始终都是当前执行的代码所在环境的变量对象。如果这个环境是函数，则将其**活动对象**（activation object）作为变量对象。活动对象在最开始时只包含一个变量，即 `arguments` 对象（这个对象在全局环境中是不存在的）。作用域链中的下一个变量对象来自包含（外部）环境，而再下一个变量对象则来自下一个包含环境。这样，一直延续到全局执行环境；全局执行环境的变量对象始终都是作用域链中的最后一个对象。

标识符解析是沿着作用域链一级一级地搜索标识符的过程。搜索过程始终从作用域链的前端开始，然后逐级地向后回溯，直至找到标识符为止（如果找不到标识符，通常会导致错误发生）。

请看下面的示例代码：

```javascript
var color = "blue";

function changeColor(){
    if (color === "blue"){
        color = "red";
    } else {
        color = "blue";
    }
}

console.log(changeColor());
```

在这个简单的例子中，函数 `changeColor()` 的作用域链包含两个对象：它自己的变量对象（其中定义着 `arguments` 对象）和全局环境的变量对象。可以在函数内部访问变量 `color`，就是因为可以在这个作用域链中找到它。

此外，在局部作用域中定义的变量可以在局部环境中与全局变量互换使用，如下面这个例子所示：

```javascript
var color = "blue";

function changeColor(){
    var anotherColor = "red";

    function swapColors(){
        var tempColor = anotherColor;
        anotherColor = color;
        color = tempColor;

        // 这里可以访问color、anotherColor和tempColor
    }

    // 这里可以访问color和anotherColor，但不能访问tempColor
    swapColors();
}

// 这里只能访问color
changeColor();
```

以上代码共涉及3个执行环境：全局环境、`changeColor()` 的局部环境和 `swapColors()` 的局部环境。全局环境中有一个变量 `color` 和一个函数 `changeColor()`。`changeColor()` 的局部环境中有一个名为 `anotherColor` 的变量和一个名为 `swapColors()` 的函数，但它也可以访问全局环境中的变量 `color`。`swapColors()` 的局部环境中有一个变量 `tempColor`，该变量只能在这个环境中访问到。无论全局环境还是 `changeColor()` 的局部环境都无权访问 `tempColor`。然而，在 `swapColors()` 内部则可以访问其他两个环境中的所有变量，因为那两个环境是它的父执行环境。下图形象地展示了前面这个例子的作用域链。

![](http://qiniu.shijiajie.com/blog/javascript-lesson/2.41.jpg)

上图中的矩形表示特定的执行环境。其中，内部环境可以通过作用域链访问所有的外部环境，但外部环境不能访问内部环境中的任何变量和函数。这些环境之间的联系是线性、有次序的。每个环境都可以向上搜索作用域链，以查询变量和函数名；但任何环境都不能通过向下搜索作用域链而进入另一个执行环境。对于这个例子中的 `swapColors()` 而言，其作用域链中包含3个对象：`swapColors()` 的变量对象、`changeColor()` 的变量对象和全局变量对象。`swapColors()` 的局部环境开始时会先在自己的变量对象中搜索变量和函数名，如果搜索不到则再搜索上一级作用域链。`changeColor()` 的作用域链中只包含两个对象：它自己的变量对象和全局变量对象。这也就是说，它不能访问 `swapColors()` 的环境。函数参数也被当作变量来对待，因此其访问规则与执行环境中的其他变量相同。

## 闭包

MDN 对闭包的定义：

> 闭包是指那些能够访问独立（自由）变量的函数（变量在本地使用，但定义在一个封闭的作用域中）。换句话说，这些函数可以「记忆」它被创建时候的环境。

《JavaScript 权威指南(第6版)》对闭包的定义：

> 函数对象可以通过作用域链相互关联起来，函数体内部的变量都可以保存在函数作用域内，这种特性在计算机科学文献中称为闭包。

《JavaScript 高级程序设计(第3版)》对闭包的定义：

> 闭包是指有权访问另一个函数作用域中的变量的函数。

上面这些定义都很晦涩难懂，[阮一峰](http://www.ruanyifeng.com/)的解释稍微好理解一些：

> 由于在 Javascript 语言中，只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成定义在一个函数内部的函数。

### 闭包的用途

闭包可以用在许多地方。它的最大用处有两个，一个是可以读取函数内部的变量（作用域链），另一个就是让这些变量的值始终保持在内存中。怎么来理解这句话呢？请看下面的代码。

```javascript
function fun() {　　　
    var n = 1;

    add = function() {
        n += 1
    }

    function fun2(){
        console.log(n);
    }

    return fun2;
}

var result = fun();　　
result(); // 1
add();
result(); // 2
```


在这段代码中，`result` 实际上就是函数 `fun2`。它一共运行了两次，第一次的值是 `1`，第二次的值是 `2`。这证明了，函数 `fun` 中的局部变量 `n` 一直保存在内存中，并没有在 `fun` 调用后被自动清除。

为什么会这样呢？原因就在于 `fun` 是 `fun2` 的父函数，而 `fun2` 被赋给了一个全局变量，这导致 `fun2` 始终在内存中，而 `fun2` 的存在依赖于 `fun`，因此 `fun` 也始终在内存中，不会在调用结束后，被垃圾回收机制（garbage collection）回收。

这段代码中另一个值得注意的地方，就是 `add = function() { n += 1 }` 这一行。首先，变量 `add` 前面没有使用 `var` 关键字，因此 `add` 是一个全局变量，而不是局部变量。其次，`add` 的值是一个匿名函数（anonymous function），而这个匿名函数本身也是一个闭包，和 `fun2` 处于同一作用域，所以 `add` 相当于是一个 `setter`，可以在函数外部对函数内部的局部变量进行操作。

###  计数器的困境

我们再来看一个经典例子「计数器的困境」，假设你想统计一些数值，且该计数器在所有函数中都是可用的。你可以定义一个全局变量 `counter` 当做计数器，再定义一个 `add()` 函数来设置计数器递增。代码如下：

```javascript
var counter = 0;
function add() {
    counter += 1;
}

console.log(add());
console.log(add());
console.log(add());
// 计数器现在为 3
```

计数器数值在执行 `add()` 函数时发生变化。但问题来了，页面上的任何脚本都能改变计数器 `counter`，即便没有调用 `add()` 函数。如果我们将计数器 `counter ` 定义在 `add()` 函数内部，就不会被外部脚本随意修改到计数器的值了。代码如下：

```javascript
function add() {
    var counter = 0;
    counter += 1;
}

console.log(add());
console.log(add());
console.log(add());
// 本意是想输出 3, 但事与愿违，输出的都是 1 
```

因为每次调用 `add()` 函数，计数器都会被重置为 0，输出的都是 1，这并不是我们想要的结果。闭包正好可以解决这个问题，我们在 `add()` 函数内部，再定义一个 `plus()` 内嵌函数（闭包），内嵌函数 `plus()` 可以访问父函数的 `counter` 变量。代码如下：

```javascript
function add() {
    var counter = 0;
    var plus = function() {counter += 1;}
    plus();    
    return counter; 
}
```

接下来，只要我们能在外部访问 `plus()` 函数，并且确保 `counter = 0` 只执行一次，就能解决计数器的困境。代码如下：

```javascript
var add = function() {
    var counter = 0;
    var plus = function() {return counter += 1;}
    return plus;
}

var puls2 = add();
console.log(puls2());
console.log(puls2());
console.log(puls2());
// 计数器为 3
```

计数器 `counter` 受 `add()` 函数的作用域保护，只能通过 `puls2` 方法修改。

### 使用闭包的注意点

- 由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在 IE 中可能导致内存泄露。解决方法是，在退出函数之前，将不使用的局部变量全部删除。
- 闭包会在父函数外部，改变父函数内部变量的值。所以，如果你把父函数当作对象（object）使用，把闭包当作它的公用方法（public method），把内部变量当作它的私有属性（private value），这时一定要小心，不要随便改变父函数内部变量的值。

JavaScript 闭包是一种强大的语言特性。通过使用这个语言特性来隐藏变量，可以避免覆盖其他地方使用的同名变量，理解闭包有助于编写出更有效也更简洁的代码。

##  扩展阅读

> 「五句话搞定 JavaScript 作用域」  
> http://www.cnblogs.com/wupeiqi/p/5649402.html

> 「JavaScript 闭包究竟是什么」  
> http://www.cnblogs.com/dolphinX/archive/2012/09/29/2708763.html

> 「阮一峰的网络日志 - 学习 JavaScript 闭包」  
> http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html

> 「王福朋 - 深入理解 JavaScript 原型和闭包（15）」  
> http://www.cnblogs.com/wangfupeng1988/p/3994065.html

> 「MDN - 闭包」  
> https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Closures

> 「菜鸟教程 - JavaScript 闭包」  
> http://www.runoob.com/js/js-function-closures.html


## 关卡

下面代码块会输出什么结果？

```javascript
// 挑战一
scope = 'stone';

function Func() {
    var scope = "sophie";

    function inner() {
        console.log(scope);
    }
    return inner;
}

var ret = Func();
ret();    // ???
```

```javascript
// 挑战二
scope = 'stone';

function Func() {
    var scope = "sophie";

    function inner() {
        console.log(scope);
    }
    scope = 'tommy';
    return inner;
}

var ret = Func();
ret();    // ???
```

```javascript
// 挑战三
scope = 'stone';

function Bar() {
    console.log(scope);
}

function Func() {
    var scope = "sophie";
    return Bar;
}

var ret = Func();
ret();    // ???
```

```javascript
// 关卡四
var name = "The Window";　　
var object = {　　　　
    name: "My Object",
    getNameFunc: function() {　　　　　　
        return function() {　　　　　　　　
            return this.name;　　　　　　
        };　　　　
    }　　
};　　
console.log(object.getNameFunc()());    // ???
```

```javascript
// 关卡五
var name = "The Window";　　
var object = {　　　　
    name: "My Object",
    getNameFunc: function() {　　　　　　
        var that = this;　　　　　　
        return function() {　　　　　　　　
            return that.name;　　　　　　
        };　　　　
    }　　
};　　
console.log(object.getNameFunc()());    // ???
```

## 更多

> 关注微信公众号「劼哥舍」回复「答案」，获取关卡详解。  
> 关注 [https://github.com/stone0090/javascript-lessons](https://github.com/stone0090/javascript-lessons)，获取最新动态。