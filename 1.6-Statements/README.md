# 语句

表达式在 JavaScript 中是短语，那么语句就是整句命令。表达式用来计算出一个值，语句用来执行以使某件事发生。从本质上看，语句定义了 JavaScript 中的主要语法，语句通常使用一或多个关键字来完成给定任务。语句可以很简单，例如通知函数退出；也可以比较复杂，例如指定重复执行某个命令的次数。下表列出了 JavaScript 大部分语句的语法和用途：

| 语句 | 语法 | 用途 |
| --- | --- | --- |
| `break` | `break [label];` | 退出最内层循环或者退出 `switch` 语句，又或者退出 `label` 指定的语句 |
| `case` | `case expression:` | 在 `switch` 语句中标记一条语句 |
| `continue` | `continue [label];` | 重新开始最内层的循环或重新开始 `label` 指定的循环 |
| `debugger` | `debugger;` | 断点器调试 |
| `default` | `default;` | 在 `switch` 中标记默认的语句 |
| `do-while` | `do statement while(expression);` | `while` 循环的一种替代形式 |
| `empty` | `;` | 什么都不做 |
| `for` | `for(init;expr;incr) statement` | 简写的循环结构 |
| `for-in` | `for(var in object) statement` | 遍历一个对象的属性 |
| `function` | `function name([param[],...])`<br>`{statement}` | 声明一个函数 |
| `if-else` | `if (expression) statement1`<br>`[else statement2]` | 执行 `statement1` 或者 `statement2` |
| `label` | `label:statement` | 给 `statement` 指定一个名字 `label` |
| `return` | `return [expression];` | 从函数返回一个值 |
| `switch` | `switch(expression){statement}` | 用 `case` 或者 `default` 语句标记的多分支语句 |
| `throw` | `throw expression;` | 抛出异常 |
| `try` | `try {statement}`<br>`[catch {handler statement}]`<br>`[finally {cleaup statement}]` | 捕获异常 |
| `use strict` | `"use strict"` | 对脚本和函数应用严格模式 |
| `var` | `var name=[=expr][,...];` | 声明并初始化一个或多个变量 |
| `while` | `while(expression) statement` | 基本的循环结构 |
| `with` | `with(object) statement` | 扩展作用域链 |

## 条件语句

### `if-else` 语句

大多数编程语言中最为常用的一个语句就是 `if-else` 语句。以下是 `if-else` 语句的语法：

``` javascript
if (condition) statement1 [else statement2]
```

其中的 `condition` 可以是任意表达式；而且对这个表达式求值的结果不一定是布尔值。JavaScript 会自动调用 `Boolean()` 转换函数将这个表达式的结果转换为一个布尔值。如果对 `condition` 求值的结果是 `true`，则执行 `statement1`，如果对 `condition` 求值的结果是 `false`，则执行 `statement2`。而且这两个语句既可以是一行代码，也可以是一个代码块（以一对花括号括起来的多行代码）。请看下面的例子：


``` javascript
if (i > 25)
    console.log("Greater than 25.");              // 单行语句
else {
    console.log("Less than or equal to 25.");     // 代码块中的语句
}
```

业界普遍推崇的最佳实践是始终使用代码块，即使要执行的只有一行代码。因为这样可以消除人们的误解，否则可能让人分不清在不同条件下要执行哪些语句。

### `switch` 语句

`switch` 语句与 `if` 语句的关系最为密切，而且也是在其他语言中普遍使用的一种流控制语句。JavaScript 中 `switch` 语句的语法与其他基于 C 的语言非常接近，如下所示：

``` javascript
switch (expression) {
  case value: statement
    break;
  case value: statement
    break;
  case value: statement
    break;
  case value: statement
    break;
  default: statement
}
```

`switch` 语句中的每一种情形的含义是：“如果表达式等于这个值（value），则执行后面的语句（statement）”。而 `break` 关键字会导致代码执行流跳出 `switch` 语句。如果省略 `break` 关键字，就会导致执行完当前 `case` 后，继续执行下一个 `case`。最后的 `default` 关键字则用于在表达式不匹配前面任何一种情形的时候，也相当于一个 `else` 语句。从根本上讲，`switch` 语句就是为了让开发人员免于编写像下面这样的代码：

``` javascript
if (i === 25){
  console.log("25");
} else if (i === 35) {
  console.log("35");
} else if (i === 45) {
  console.log("45");
} else {
  console.log("Other");
}
```

而与此等价的switch语句如下所示：

``` javascript
switch (i) {
    case 25: 
        console.log("25");
        break;
    case 35: 
        console.log("35");
        break;
    case 45: 
        console.log("45");
        break;
    default: 
        console.log("Other");
}
```

通过为每个case后面都添加一个break语句，就可以避免同时执行多个case代码的情况。假如确实需要混合几种情形，不要忘了在代码中添加注释，说明你是有意省略了break关键字。

虽然 JavaScript 中的 `switch` 语句借鉴自其他语言，但这个语句也有自己的特色。首先，可以在 `switch` 语句中使用任何数据类型（在很多其他语言中只能使用数值），无论是字符串，还是对象都没有问题。其次，每个 `case` 的值不一定是常量，可以是变量，甚至是表达式。请看下面这两个例子：

``` javascript
switch ("hello world") {
    case "hello" + " world": 
        console.log("Greeting was found.");
        break;
    case "goodbye": 
        console.log("Closing was found.");
        break;
    default: 
        console.log("Unexpected message was found.");
}

var num = 25;
switch (true) {
    case num < 0: 
        console.log("Less than 0.");
        break;
    case num >= 0 && num <= 10: 
        console.log("Between 0 and 10.");
        break;
    case num > 10 && num <= 20: 
        console.log("Between 10 and 20.");
        break;
    default: 
        console.log("More than 20.");
}
```

`switch` 语句首先计算 `switch` 关键字后的表达式，然后按照从上到下的顺序计算每个 `case` 后的表达式，直到执行到 `case` 的表达式的值与 `switch` 的表达式的值相等时为止。由于对每个 `case` 的匹配操作实际上是 `===` 恒等运算符比较，而不是 `==` 相等运算符比较，因此，表达式和 `case` 的匹配并不会做任何类型转换。

## 循环

### `while` 语句

`while` 语句属于前测试循环语句，也就是说，在循环体内的代码被执行之前，就会对出口条件求值。因引，循环体内的代码有可能永远不会被执行。以下是 `while` 语句的语法：

``` javascript
while(expression) statement
```

下面是一个示例：

``` javascript
var i = 0;
while (i < 10) {
    i += 2;
}
```

### `do-while` 语句

`do-while` 语句是一种后测试循环语句，即只有在循环体中的代码执行之后，才会测试出口条件。换句话说，在对条件表达式求值之前，循环体内的代码至少会被执行一次。以下是 `do-while` 语句的语法：

``` javascript
do {
    statement
} while (expression);
```

下面是一个示例：

``` javascript
var i = 0;
do {
   i += 2;
} while (i < 10);
```

### `for` 语句

`for` 语句也是一种前测试循环语句，但它具有在执行循环之前初始化变量和定义循环后要执行的代码的能力。以下是 `for` 语句的语法：

``` javascript
for (initialization; expression; post-loop-expression) statement
```

下面是一个示例：
``` javascript
var count = 10;
for (var i = 0; i < count; i++){
    console.log(i);
}
```
这个 `for` 循环语句与下面的 `while` 语句的功能相同：


``` javascript
var count = 10;
var i = 0;
while (i < count){
    console.log(i);
    i++;
}
```

由于 JavaScript 中不存在块级作用域，因此在循环内部定义的变量也可以在外部访问到。例如：

``` javascript
var count = 10;
for (var i = 0; i < count; i++){
    console.log(i);
}
console.log(i); // 10
```

此外，`for` 语句中的初始化表达式、控制表达式和循环后表达式都是可选的。将这两个表达式全部省略，就会创建一个无限循环，例如：

``` javascript
// 无限循环
for (;;) {
    doSomething();
}

```

### `for-in` 语句

`for-in` 语句是一种精准的迭代语句，可以用来枚举对象的属性。以下是 `for-in` 语句的语法：

``` javascript
for (property in object) statement
```

下面是一个示例：

``` javascript
for (var propName in window) {
    console.log(propName);
}
```

在这个例子中，我们使用 `for-in` 循环来显示了 BOM 中 `window` 对象的所有属性。每次执行循环时，都会将 `window` 对象中存在的一个属性名赋值给变量 `propName`。这个过程会一直持续到对象中的所有属性都被枚举一遍为止。与 `for` 语句类似，这里控制语句中的 `var` 操作符也不是必需的。但是，为了保证使用局部变量，我们推荐上面例子中的这种做法。

JavaScript 对象的属性没有顺序。因此，通过 `for-in` 循环输出的属性名的顺序是不可预测的。具体来讲，所有属性都会被返回一次，但返回的先后次序可能会因浏览器而异。

如果表示要迭代的对象的变量值为 `null` 或 `undefined`，`for-in` 语句会抛出错误。虽然 ECMAScript 5更正了这一行为；对这种情况不再抛出错误，而只是不执行循环体。为了保证最大限度的兼容性，建议在使用 `for-in` 循环之前，先检测确认该对象的值不是 `null` 或 `undefined`。

## 跳转

### `label` 语句

使用 `label` 语句可以在代码中添加标签，以便将来使用。以下是 `label` 语句的语法：

``` javascript
label: statement
```

下面是一个示例：

``` javascript
start: for (var i=0; i < count; i++) {
    console.log(i); 
}
```

这个例子中定义的 `start` 标签可以在将来由 `break` 或 `continue` 语句引用。加标签的语句一般都要与 `for` 语句等循环语句配合使用。

### `break` 和 `continue` 语句

`break` 和 `continue` 语句用于在循环中精确地控制代码的执行。其中，`break` 语句会立即退出循环，强制继续执行循环后面的语句。而 `continue` 语句虽然也是立即退出循环，但退出循环后会从循环的顶部继续执行。请看下面的例子：

``` javascript
var num = 0;

for (var i=1; i < 10; i++) {
    if (i % 5 == 0) {
       break;
    }
    num++;
}

console.log(num);   // 4
```

这个例子中的 `for` 循环会将变量 `i` 由1递增至 `10`。在循环体内，有一个 `if` 语句检查 `i` 的值是否可以被 `5` 整除（使用求模运算符）。如果是，则执行 `break` 语句退出循环。另一方面，变量 `num` 从 `0` 开始，用于记录循环执行的次数。在执行 `break` 语句之后，结果显示 `4`。也就是说，在变量 `i` 等于 `5` 时，循环总共执行了 `4` 次；而 `break` 语句的执行，导致了循环在 `num` 再次递增之前就退出了。如果在这里把 `break` 替换为 `continue` 的话，则可以看到另一种结果：

``` javascript
var num = 0;

for (var i=1; i < 10; i++) {
if (i % 5 == 0) {
        continue;
    }
    num++;
}

console.log(num);   // 8
```

例子的结果显示 `8`，也就是循环总共执行了 `8` 次。当变量 `i` 等于 `5` 时，循环会在 `num` 再次递增之前退出，但接下来执行的是下一次循环，即i的值等于 `6` 的循环。于是，循环又继续执行，直到 `i` 等于 `10` 时自然结束。而 `num` 的最终值之所以是 `8`，是因为 `continue` 语句导致它少递增了一次。

`break` 和 `continue` 语句都可以与 `label` 语句联合使用，从而返回代码中特定的位置。这种联合使用的情况多发生在循环嵌套的情况下，如下面的例子所示：

``` javascript
var num = 0;

outermost:
for (var i = 0; i < 10; i++) {
     for (var j = 0; j < 10; j++) {
        if (i == 5 && j == 5) {
            break outermost;
        }
        num++;
    }
}

console.log(num);   // 55
```


在这个例子中，`outermost` 标签表示外部的 `for` 语句。如果每个循环正常执行 `10` 次，则 `num++` 语句就会正常执行 `100` 次。换句话说，如果两个循环都自然结束，`num` 的值应该是 `100`。但内部循环中的 `break` 语句带了一个参数：要返回到的标签。添加这个标签的结果将导致 `break` 语句不仅会退出内部的 `for` 语句（即使用变量 `j` 的循环），而且也会退出外部的 `for` 语句（即使用变量 `i` 的循环）。为此，当变量 `i` 和 `j` 都等于 `5` 时， `num`的值正好是 `55`。同样，`continue` 语句也可以像这样与 `label` 语句联用，如下面的例子所示：

``` javascript
var num = 0;

outermost:
for (var i = 0; i < 10; i++) {
    for (var j = 0; j < 10; j++) {
        if (i == 5 && j == 5) {
            continue outermost;
        }
        num++;
    }
}

console.log(num);   // 95
```

在这种情况下，`continue` 语句会强制继续执行循环，退出内部循环，执行外部循环。当 `j` 是 `5` 时，`continue` 语句执行，而这也就意味着内部循环少执行了 `5` 次，因此 `num` 的结果是 `95`。

虽然联用 `break`、`continue` 和 `label` 语句能够执行复杂的操作，但如果使用过度，也会给调试带来麻烦。在此，我们建议如果使用 `label` 语句，一定要使用描述性的标签，同时不要嵌套过多的循环。

### `return` 语句

`return` 语句的作用是指定函数调用后的返回值。`return` 语句的语法如下：

``` javascript
return [expression];
```

下面是一个示例：

``` javascript
function square(x) { return x*x; }  // 一个包含 return 语句的函数
square(2);                          // 调用结果为 4
```

`return` 语句只能在函数体内出现，如果不是的话会报语法错误。当执行到 `return` 语句的时候，函数终止执行，并返回 `expression` 的值给调用程序。如果没有 `return` 语句，则函数调用仅依次执行函数体内的每一条语句直到函数结束，最后返回调用程序。这种情况下，调用表达式的结果是 `undefined`。`return` 语句经常作为函数内的最后一条语句出现，但并不是说要一定放在函数最后。`return` 语句可以单独使用而不必带有 `expression`，这样的话函数也会向调用程序返回 `undefined`。

由于 JavaScript 可以自动插入分号，因此在 `return` 关键字和它后面的表达式之间不能有换行。

### `throw` 语句

`throw` 语句的作用是把程序运行时产生的错误显式地抛出异常。`throw` 语句的语法如下：

``` javascript
throw expression;
```

`expression` 的值可以是任意类型的。可以抛出一个代表错误码的数字，或者包含可读的错误消息的字符串。当 JavaScript 解释器抛出异常的时候通常采用 `Error` 类型和其子类型。`Error` 对象有一个 `name` 属性表示错误类型，一个 `message` 属性用来存放传递给构造函数的字符串，在下面的例子中，当使用非法参数调用函数时就抛出一个 `Error` 对象：

``` javascript
function factorial(x) {
// 如果输入参数是非法的，则抛出一个异常
    if (x < 0) throw new Error("x不能是负数");
    // 否则，计算出一个值，并正常地返回它
    for(var f = 1; x > 1; f *= x, x--) /* empty */ ;
    return f;
}
```

当抛出异常时，JavaScript 解释器会立即停止当前正在执行的逻辑，并跳转至就近的异常处理程序。如果没有找到任何异常处理程序，异常就会沿着 JavaScript 方法的词法结构和调用栈向上传播。最终 JavaScript 将把异常当成程序错误来处理，并报告给用户。

### `try` 语句

`try-catch-finally` 语句是 JavaScript 中异常处理机制，`try-catch-finally` 语句的语法如下：

``` javascript
try {statement} [catch {handler statement}] [finally {cleaup statement}]
```

`try` 从句定义了需要处理的异常所在的代码块。`catch` 从句跟随在 `try` 从句之后，当 `try` 块内某处发生了异常时，调用 `catch` 内的代码逻辑。`catch` 从句后跟随 `finally` 块，后者中放置清理代码，不管 `try` 块中是否产生异常，`finally` 块内的逻辑总是会执行。尽管 `catch` 和 `finally` 都是可选的，但 `try` 从句需要至少二者之一与之组成完整的语句。`try`、`catch` 和 `finally` 语句块都需要使用花括号括起来，这里的花括号是必需的，即使从句中只有一条语句也不能省略花括号。

下面的代码详细的说明了 `try-catch-finally` 的使用目的：

``` javascript
try {
    // 通常来讲，这里的代码会从头执行到尾而不会产生任何问题，
    // 但有时会抛出一个异常，要么是由 throw 语句直接抛出异常，
    // 要么是通过调用一个方法间接抛出异常
}
catch(e) {
    // 当且仅当 try 语句块抛出了异常，才会执行这里的代码
    // 这里可以通过局部变量 e 来获得对 Error 对象或者抛出的其他值的引用
    // 这里的代码块可以基于某种原因处理这个异常，也可以忽略这个异常，
    // 还可以通过 throw 语句重新抛出异常
}
finally {
    // 不管 try 语句块是否抛出了异常，这里的逻辑总是会执行，终止 try 语句块的方式有：
    // 1）正常终止，执行完语句块的最后一条语句
    // 2）通过 break、continue 或 return 语句终止
    // 3）抛出一个异常，异常被 catch 从句捕获
    // 4）抛出一个异常，异常未被捕获，继续向上传播
}
```

## 其他

### `with` 语句

`with` 语句的作用是将代码的作用域设置到一个特定的对象中。`with` 语句的语法如下：

``` javascript
with (expression) statement;
```

定义 `with` 语句的目的主要是为了简化多次编写同一个对象的工作，如下面的例子所示：

``` javascript
var qs = location.search.substring(1);
var hostName = location.hostname;
var url = location.href;
```

上面几行代码都包含 `location` 对象。如果使用 `with` 语句，可以把上面的代码改写成如下所示：

``` javascript
with(location){
    var qs = search.substring(1);
    var hostName = hostname;
    var url = href;
}
```

在这个重写后的例子中，使用 `with` 语句关联了 `location` 对象。这意味着在 `with` 语句的代码块内部，每个变量首先被认为是一个局部变量，而如果在局部环境中找不到该变量的定义，就会查询 `location` 对象中是否有同名的属性。如果发现了同名属性，则以 `location` 对象属性的值作为变量的值。

由于大量使用 `with` 语句会导致性能下降，同时也会给调试代码造成困难，因此在开发大型应用程序时，不建议使用 `with` 语句。严格模式下不允许使用 `with` 语句，否则将视为语法错误。

### `debugger` 语句

`debugger` 语句通常什么也不做。然而，当浏览器的调试工具可用并运行的时候，JavaScript 解释器将会以调式模式运行。实际上，这条语句用来产生一个断点（breakpoint），JavaScript 代码的执行会停止在断点的位置，这时可以使用调试器输出变量的值、检查调用栈等。例如：

``` javascript
function f(o) {
    if (o === undefined) {
        debugger;  // 程序会停止在该位置
    }
    // 函数的其他部分
}
```

### `use strict` 语句

> 请参见[「语法」-「严格模式」](/1.3-Syntax.md#严格模式)。

## 关卡

``` javascript
// 挑战一
var k;
for(i=0, j=0; i<10, j<6; i++, j++){
    k = i + j;
}
console.log(k);  // ???
```

``` javascript
// 挑战二
var nums = [12,32,54,56,78,89];
for(var n in nums){
    console.log(n);  // ???
}
```

``` javascript
// 挑战三
function showCase(value) {
    switch (value) {
        case 'A':
            console.log('Case A');
            break;
        case 'B':
            console.log('Case B');
            break;
        case undefined:
            console.log('undefined');
            break;
        default:
            console.log('Do not know!');
    }
}
showCase(new String('A'));   // ???
```

``` javascript
// 挑战四
function showCase(value) {
    switch (value) {
        case 'A':
            console.log('Case A');
        case 'B':
            console.log('Case B');
            break;
        case undefined:
            console.log('undefined');
            break;
        default:
            console.log('Do not know!');
    }
}
showCase(String('A'));   // ???
```

``` javascript
// 挑战五
var i = 0;
for (;;) {
    if (i = 2) {
        continue;
    }
    if (i = 20) {
        break;
    }
    i++;
}
console.log(i);  // ???
```

<small>关注微信公众号「石佳劼的博客」，回复「答案」，即可获取关卡详解。</small>

## 更多

您还可以在 [GitHub](https://github.com/) 上 [Star](https://github.com/stone0090/javascript-lessons) 该课程，获取最新内容，查看章节目录。

> https://github.com/stone0090/javascript-lessons

![](http://7xkhp9.com1.z0.glb.clouddn.com/blog/other/blog_statement_20160618_01.png?imageView2/2/w/650/interlace/1/q/100)