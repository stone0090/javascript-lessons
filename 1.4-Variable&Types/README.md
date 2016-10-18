# 变量和数据类型

当程序需要将值保存起来以备将来使用时，便将其赋值给一个变量，值的类型称作数据类型。

## 变量

JavaScript 的变量是松散类型的，所谓松散类型就是可以用来保存任何类型的数据。换句话说，每个变量仅仅是一个用于保存值的占位符而已。定义变量时要使用关键字 `var` 来声明的，如下所示：

``` javascript
var message;
```

这行代码定义了一个名为 `message` 的变量，该变量可以用来保存任何值（像这样未经过初始化的变量，会保存一个特殊的值 `undefined`）。JavaScript 也支持直接初始化变量，因此在定义变量的同时就可以设置变量的值，如下所示：

``` javascript
var message = "hello";
```

此时，变量 `message` 中保存了一个字符串值 `"hello"`。像这样初始化变量并不会把它标记为字符串类型，因此，可以在修改变量值的同时修改值的类型。如下所示：

``` javascript
var message = "hello";
message = 100;           // 有效的语句，不好的写法
```

在这个例子中，变量 `message` 一开始保存了一个字符串值 `"hello"`，然后该值又被一个数字值100取代。虽然我们不建议修改变量所保存值的类型，但这种操作在 JavaScript 中完全有效。

有一点必须注意，即使用 `var` 运算符定义的变量是的该作用域中的局部变量。也就是说，如果在函数中使用 `var` 定义一个变量，那么这个变量在函数退出后就会被销毁，例如：

``` javascript
function test(){
    var message = "hello";  // 局部变量
}
test();
console.log(message);   // 产生错误

```

这里，变量 `message` 是在函数中使用 `var` 定义的，是局部变量。当函数被调用时，就会创建该变量并为其赋值。而在此之后，这个变量又会立即被销毁，因此例子中的下一行代码就会导致错误。不过，可以像下面这样省略 `var` 运算符，从而创建一个全局变量：

``` javascript
function test(){
    message = "hello";  // 全局变量，不好的写法
}
test();
console.log(message);   // "hello"

```

这个例子省略了 `var` 运算符，因而 `message` 就成了全局变量。这样，只要调用一次 `test()` 函数，这个变量就有了定义，就可以在函数外部的任何地方被访问到。

虽然省略 `var` 运算符可以定义全局变量，但这也不是推荐的做法，因为在局部作用域中定义全局变量很难维护，给未经声明的变量赋值在严格模式下会抛出 `ReferenceError` 错误。

## 数据类型

JavaScript 中有5种简单数据类型（也称为「基本数据类型」或「原始数据类型」）：`Undefined`、`Null`、`Boolean`、`Number`、`String` 。还有1种复杂数据类型 `Object`，`Object` 本质上是由一组无序的名值对组成的。JavaScript 不支持任何创建自定义类型的机制，所有值最终都将是上述6种数据类型之一。

### `typeof` 运算符

鉴于 JavaScript 是松散类型的，因此需要有一种手段来检测给定变量的数据类型，`typeof` 就是负责提供这方面信息的运算符。对一个值使用 `typeof` 运算符可能返回下列某个字符串：

- `"undefined"`，如果这个值未声明或已声明但未初始化。
- `"boolean"`，如果这个值是布尔值。
- `"string"`，如果这个值是字符串。
- `"number"`，如果这个值是数值。
- `"object"`，如果这个值是对象或 `null`。
- `"function"`，如果这个值是函数。

下面是几个使用 `typeof` 运算符的例子：

``` javascript
var message = "some string";
console.log(typeof message);     // "string"
console.log(typeof(message));    // "string"
console.log(typeof 95);          // "number"
```

从以上例子可以看出，`typeof` 运算符既可以对变量使用，又可以对字面量使用。由于 `typeof` 是一个运算符而不是函数，因此例子中的圆括号尽管可以使用，但并不提倡。

`typeof null` 结果是 `"object"` 是历史遗留 Bug，在 ECMAScript 6中，曾经有提案为历史平反, 将 `typeof null` 的值纠正为 `"null"`，但最后该提案被拒。理由是历史遗留代码太多，不如继续将错就错。

从技术角度讲，函数在 JavaScript 中是对象，不是一种数据类型。然而，函数也确实有一些特殊的属性，因此通过 `typeof` 运算符来区分函数和其他对象是有必要的。

> 扩展阅读「为什么 JavaScript 里面 `typeof null` 的值是 `"object"`？」  
> https://www.zhihu.com/question/21691758

> 扩展阅读「MDN 之 `typeof`」  
> https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/typeof

> 扩展阅读「JavaScript 检测原始值、引用值、属性」  
> http://shijiajie.com/2016/06/20/javascript-maintainable-javascript-validate1/

> 扩展阅读「JavaScript 检测之 basevalidate.js」  
> http://shijiajie.com/2016/06/25/javascript-maintainable-javascript-basevalidatejs/

### `Undefined` 类型

`Undefined` 类型只有1个值，即 `undefined`。使用 `var` 声明变量但未对其加以初始化时，这个变量的值就是 `undefined`，直接使用未声明的变量会产生错误。对未声明或已声明但未初始化的变量执行 `typeof` 运算符会返回 `"undefined"` 值，例如：

``` javascript
var message;    // 这个变量声明之后默认取得了 undefined 值
// var age      // 这个变量并没有声明

console.log(message);           // "undefined"
console.log(age);               // 产生错误
console.log(typeof message);    // "undefined"
console.log(typeof age);        // "undefined"
```

### `Null` 类型

`Null` 类型也只有1个值，即 `null`。它用来表示值的空缺。你可以认为 `undefined` 是表示系统级的、出乎意料的或类似错误的值的空缺，而 `null` 是表示程序级的、正常的或在意料之中的值的空缺。在下列场景中应当使用 `null`。

- 用来初始化一个变量，这个变量可能赋值为一个对象。
- 用来和一个已经初始化的变量比较，这个变量可以是也可以不是一个对象。
- 当函数的参数期望是对象时，作用参数传入。
- 当函数的返回值期望是对象时，作用返回值传出。

在下列场景中不应当使用 `null`。

- 不要使用 `null` 来检测是否传入了某个参数。
- 不要使用 `null` 来检测一个未初始化的变量。

### `Boolean` 类型

`Boolean` 类型是 JavaScript 中使用得最多的一种类型，该类型只有两个字面值：`true` 和 `false`。需要注意的是，他们是区分大小写的，也就是说 `True` 和 `False`（以及其他的混合大小写形式）都不是 `Boolean` 值，只是标识符。

虽然 `Boolean` 类型的字面值只有两个，但 JavaScript 中所有类型的值都能使用 `if` 语句或 `Boolean()` 函数转换为对应的 `Boolean` 值，例如：

``` javascript
var message = "Hello world!";
if (message){
    console.log("Value is true.");  // 被执行
}
var messageAsBoolean = Boolean(message);
console.log(messageAsBoolean);  // true
```

下表给出了各种数据类型及其对应的转换规则。

| 数据类型      | 转换为true的值      | 转换为false的值 |
| --------- | -------------- | ---------- |
| Undefined | -              | undefined  |
| Null      | -              | null       |
| Boolean   | true           | false      |
| String    | 任何非空字符串        | ""（空字符串）   |
| Number    | 任何非零数字值（包括无穷大） | 0和NaN      |
| Object    | 任何对象           | -          |

### `Number` 类型

`Number` 类型是 JavaScript 中最令人关注的数据类型，这种类型使用 IEEE 754 格式来表示整数和浮点数值（浮点数值在某些语言中也被称为双精度数值）。和其他编程语言不同，JavaScript 中的所有数字均用浮点数值表示。

> 扩展阅读「IEEE 754-1985」  
> https://en.wikipedia.org/wiki/IEEE_754-1985


#### 整数

在 JavaScript 中进行算术计算时，所有以八进制和十六进制表示的数值最终都将被转换成十进制数值。例如：


``` javascript
var a = 10;         // 十进制
var b = 023;        // 八进制
var c = 0x12ac;     // 十六进制
console.log(b);     // 19
console.log(c);     // 4780
```

八进制第一位必须是0，后面跟八进制序列0到7，如果超出了范围，则忽略前导0，后面的数值当做十进制解析，例如：089会被解析为89。（八进制字面量在严格模式下是无效的，会抛出错误。）

十六进制前两位必须是 0x 或 0X，后跟十六进制序列0~9、a~f（不区分大小写），如果超出了范围，则会报语法错误。

#### 浮点数

所谓浮点数值，就是该数值中必须包含一个小数点，并且小数点后面必须至少有一位数字。虽然小数点前面可以没有整数，但我们不推荐这种写法。例如：

``` javascript
var a = 1.1;
var b = 0.1;
var c = .1;     // 有效，但不推荐
```

JavaScript 会不失时机的将浮点数转换成整数。例如：

``` javascript
var a = 5.;      // 解析成整数5
var b = 5.0;     // 解析成整数5
```

对于极大或者极小的数值，可采用科学技术法（也称e表示法）。JavaScript 会将那些小数点后面带有6个零以上的小于1的浮点数值转换为以e表示法表示的数值。例如：

``` javascript
var a = 3.14e7;             // 等于31400000
var b = 3.14E-7;            // 等于0.000000314
console.log(0.0000003);     // 3e-7
```

浮点数值的最高精度是17位小数，但在进行算术计算时其精确度远远不如整数，例如：

``` javascript
console.log(0.1 + 0.2);     // 0.30000000000000004
```

这个舍入误差会导致无法测试特定的浮点数值，因此，**永远不要测试某个特定的浮点数值**。

#### 正无穷、负无穷

由于内存限制，JavaScript 能表示的数值范围从 `Number.MIN_VALUE` 到 `Number.MAX_VALUE`，并将超出范围的数转换成 `Number.POSITIVE_INFINITY` 或 `Number.NEGATIVE_INFINITY`。0作为除数是不会报错的，正数除以0返回正无穷，负数除以0返回负无穷，0除以0返回`NaN`。例如：

``` javascript
console.log(Number.MAX_VALUE);       // 最大数 1.7976931348623157e+308
console.log(Number.MIN_VALUE);       // 最小数 5e-324

console.log(Number.POSITIVE_INFINITY);    // 正无穷  Infinity
console.log(Number.NEGATIVE_INFINITY);    // 负无穷 -Infinity

console.log( 1 / 0);     //  Infinity
console.log(-1 / 0);     // -Infinity

```

JavaScript 提供了 `isFinite()` 函数，来确定一个数是不是有穷的。例如：

``` javascript
console.log(isFinite(100));         // true
console.log(isFinite(Infinity));    // false 
```

#### NaN

`NaN`（not a number），是一个特殊的数值。之所以称它为「非数值」，是因为它不能参与算数运算，任何涉及 `NaN` 的操作都返回 `NaN`。并且 `NaN` 与任何值都不相等（包括自身）。例如：

``` javascript
console.log(typeof NaN);      // "number"

console.log(0 / 0);                 // NaN
console.log(NaN - NaN);             // NaN
console.log(Infinity - Infinity);   // NaN

var a = NaN;
console.log(a === a);   // false
```

JavaScript 提供了 `isNaN()` 函数，来确定一个数是不是 `NaN`。例如：

``` javascript
console.log(isNaN(100));        //  false
console.log(isNaN("100"));      //  false
console.log(isNaN(true));       //  false
console.log(isNaN("sss"));      //  true
console.log(isNaN(NaN));        //  true
```

#### `Number()`、`parseInt()`、`parseFloat()` 转型函数

`isNaN()` 函数在接收到一个值之后，会尝试使用转型函数 `Number()` 将这个值转换为数值，转换规则如下：

- `undefined` 转换为 `NaN`；
- `null` 转换为 0；
- `true` 转换为 `1`、`false` 转换为 `0`；
- `number` 整数转换为十进制，小数不变；
- `string` 如果只包含十进制数和小数，则返回对应的数值，如果只包含八进制数，则忽略前导0返回剩余部分，如果只包含十六进制，则返回十进制数，空字符串转换为0，其它字符串转换为 `NaN`；
- 如果 `object` 具有 `valueOf()` 方法，且返回一个原始值（5种简单数据类型），则将这个原始值转换为数字，并返回这个数字；否则，如果 `object` 具有 `toString()` 方法，且返回一个原始值，则将这个原始值转换为数字，并返回这个数字；否则，抛出一个类型错误异常。

由于 `Number()` 转型函数在转换字符串时不够理想，因此还有两个专门用来转换字符串的函数 `parseInt()` 和 `parseFloat()` 函数。

`parseInt()` 函数会忽略字符串前面的空格，直至找到第一个非空格字符，只要第一个非空格字符不是数字或者正负号，一律返回 `NaN`， 如果第一个非空格字符是数字字符，`parseInt()` 会继续解析第二个字符，直到解析完所有后续字符或者遇到了一个非数字字符。例如：

``` javascript
console.log(parseInt(""));          // NaN（Number("")返回 0）
console.log(parseInt("123S"));      // 123
console.log(parseInt("12.4"));      // 12
```

`parseFloat()` 函数也会忽略字符串前面的空格，直至找到第一个非空格字符，只要第一个非空格字符不是数字或者正负号或者小数点，一律返回 `NaN`， 如果第一个非空格字符是上述字符之一，`parseFloat()` 会继续解析第二个字符，直到解析完所有后续字符或者遇到了一个非浮点数值。例如：

``` javascript
console.log(parseFloat("098.2"));       // 98.2
console.log(parseFloat("123.23.23"));   // 123.23
```

### `String` 类型

`String` 类型用于表示由零或多个16位 Unicode 字符组成的字符序列，即字符串。字符串可以由双引号（"）或单引号（'）表示，因此下面两种字符串的写法都是有效的：

``` javascript
var firstName = "Nicholas";
var lastName = 'Zakas';
```

JavaScript 中的这两种语法形式没有什么区别。用双引号表示的字符串和用单引号表示的字符串完全相同。不过，以双引号开头的字符串也必须以双引号结尾，而以单引号开头的字符串必须以单引号结尾。

`String` 数据类型包含一些特殊的字符字面量，也叫转义序列，用于表示非打印字符，或者具有其他用途的字符。例如：`\n` 换行、`\t` 制表、`\b` 空格、`\r` 回车、`\f` 进纸、`\\` 斜杠、`\'` 单引号，在用单引号表示的字符串中使用、`\"` 双引号，在用双引号表示的字符串中使用。

转义字符可出现在字符串中的任意位置，且长度为1。如要在字符串中显示 `\` ，则必须使用 `\` 进行转义。例如：

``` javascript
console.log("\n\\".length);    // 2
console.log("\\hello");        // "\hello"（长度为6）
```

大部分值都可以使用继承而来的 `toString()`方法转换为字符串，但 `undefined` 和 `null` 值没有这个方法。对数值使用 `toString()` 方法时，可以传入一个数字基数，以此输出对应进制的字符串值。例如：

``` javascript
console.log(true.toString());   // "true"

var num = 10;
console.log(num.toString());    // "10"
console.log(num.toString(2));   // "1010"
console.log(num.toString(8));   // "12"
console.log(num.toString(16));  // "a"
```

在不知道要转换的值是不是 `undefined` 或 `null` 的情况下，还可以使用转型函数 `String()`，这个函数能够将任何类型的值转换为字符串。`String()` 函数遵循下列转换规则：

- 如果值有 `toString()` 方法，则调用该方法（没有参数）并返回相应的结果；
- 如果值是 `undefined`，则返回 `"undefined"`；
- 如果值是 `null`，则返回 `"null"`。

``` javascript
var value;
console.log(String(10));        // "10"
console.log(String(true));      // "true"
console.log(String(null));      // "null"
console.log(String(value));     // "undefined"
```

### `Object` 类型

JavaScript 中所有对象都继承自 `Object` 类型，每个对象都具有下列基本的属性和方法：

- `constructor`：保存着用于创建当前对象的函数（构造函数）。
- `hasOwnProperty()`：用于检查给定的属性在当前对象实例中是否存在。
- `propertyIsEnumerable()`：用于检查给定的属性是否能够使用for-in语句来枚举。
- `isPrototypeOf()`：用于检查对象是否是传入对象的原型。
- `toString()` 方法：返回对象的字符串表示。
- `toLocaleString()`：返回对象的本地字符串表示。
- `valueOf()`：返回对象的字符串、数值或布尔值表示（通常与toString()方法的返回值相同）。

`Object` 本质上是由一组无序的名值对组成，「名称」部分是一个 JavaScript 字符串，「值」部分可以是任何 JavaScript 的数据类型（包括对象和方法）。这使用户可以根据具体需求，创建出相当复杂的数据结构。


以下两种方法都可以创建一个空对象，这两种方法在语义上是相同的。第二种更方便的方法叫作「对象字面量」法。这也是 JSON 格式的核心语法，一般我们优先选择第二种方法。例如：

``` javascript
var obj = new Object();
var obj = {};   // 好的写法
```

「对象字面量」也可以用来在对象实例中定义一个对象：
``` javascript
var obj = {
    name: "Carrot",
    "for": "Max",
    details: {
        color: "orange",
        size: 12
    }
}
```

对象的属性可以通过链式（chain）表示方法进行访问：

``` javascript
obj.details.color;       // orange
obj["details"]["size"];  // 12
```

完成创建后，对象属性可以通过如下两种方式进行赋值和访问：

``` javascript
obj.name = "Simon"      // 赋值
var name = obj.name;    // 访问

obj["name"] = "Simon";  // 赋值
var name = obj["name"]; // 访问
```

## 关卡

``` javascript
// 挑战一
console.log(typeof "undefined");  // ???
console.log(typeof null);         // ???
```

``` javascript
// 挑战二
var message = "some string";
console.log(typeof massage);    // ???
message = 10000;
console.log(typeof message);    // ???
```

``` javascript
// 挑战三
var a;
var b = null;
var c = {};
if(a && b && c){
    console.log("true.");       // ???
}else{
    console.log("false.");      // ???
}
```

``` javascript
// 挑战四
console.log(typeof (0 / 0));    // ???
console.log(023 + 123);         // ???
```

``` javascript
// 挑战五
console.log(Number("1234S"));   // ???
console.log(parseInt("1234S")); // ???
```

``` javascript
// 挑战六
console.log(3.14E-7 === 0.000000314);   // ???
console.log(0.1 + 0.6 === 0.7); // ???
console.log(0.1 + 0.7 === 0.8); // ???
console.log(NaN === NaN);       // ???
```

``` javascript
// 挑战七
console.log("\right\now");          // ???
console.log("\right\now".length);   // ???
console.log(010.toString(2));       // ???
```

``` javascript
// 挑战八
// 1、为 person、wife、child 对象新增 weight 属性，数值分别为 62、36、15。
// 2、为 person 对象新增二胎 child2 子对象，name 为 emma，其他属性自行发挥。
var person = {
    name: "stone",
    age: 30,
    wife: {
        name: "sohpie",
        age: 30
    },
    child:{
        name: "tommy",
        age: 3
    }
}
```

> 挑战九，深度阅读下面两篇文章，提出你的疑问。
>
> 「JavaScript 检测原始值、引用值、属性」
> http://shijiajie.com/2016/06/20/javascript-maintainable-javascript-validate1/
>
> 「JavaScript 检测之 basevalidate.js」
> http://shijiajie.com/2016/06/25/javascript-maintainable-javascript-basevalidatejs/

## 更多

> 关注微信公众号「劼哥舍」回复「答案」，获取关卡详解。  
> 关注 [https://github.com/stone0090/javascript-lessons](https://github.com/stone0090/javascript-lessons)，获取最新动态。

