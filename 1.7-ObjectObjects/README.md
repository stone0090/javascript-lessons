# 对象

对象是 JavaScript 的数据类型。它将很多值（原始值或者其他对象）聚合在一起，可通过名字访问这些值，因此我们可以把它看成是从字符串到值的映射。对象是动态的，可以随时新增和删除自有属性。对象除了可以保持自有的属性，还可以从一个称为原型的对象继承属性，这种「原型式继承（prototypal inheritance）」是 JavaScript 的核心特征。

对象最常见的用法是创建（create）、设置（set）、查找（query）、删除（delete）、检测（test）和枚举（enumerate）它的属性。

属性包括名字和值。属性名可以是包含空字符串在内的任意字符串，但对象中不能存在两个同名的属性。值可以是任意 JavaScript 值，或者在 ECMAScript 5中可以是 `getter` 或 `setter` 函数。

除了名字和值之外，每个属性还有一些与之相关的值，称为「属性特性（property attribute）」：

- 可写（writable attribute），表明是否可以设置该属性的值。
- 可枚举（enumerable attribute），表明是否可以通过 `for-in` 循环返回该属性。
- 可配置（configurable attribute），表明是否可以删除或修改该属性。

在 ECMAScript 5之前，通过代码给对象创建的所有属性都是可写的、可枚举的和可配置的。在 ECMAScript 5中则可以对这些特性加以配置。

除了包含属性特性之外，每个对象还拥有三个相关的「对象特性（object attribute）」：

- 对象的类（class），是一个标识对象类型的字符串。
- 对象的原型（prototype），指向另外一个对象，本对象的属性继承自它的原型对象。
- 对象的扩展标记（extensible flag），指明了在 ECMAScript 5中是否可以向该对象添加新属性。

最后，用下面术语来对 JavaScript 的「三类对象」和「两类属性」进行区分：

- 内置对象（native object），是由 JavaScript 规范定义的对象或类。例如，数组、函数、日期和正则表达式都是内置对象。
- 宿主对象（host object），是由 JavaScript 解释器所嵌入的宿主环境（比如 Web 浏览器）定义的。客户端 JavaScript 中表示网页结构的 HTMLElement 对象均是宿主对象。
- 自定义对象（user-defined object），是由运行中的 JavaScript 代码创建的对象。
- 自有属性（own property），是直接在对象中定义的属性。
- 继承属性（inherited property），是在对象的原型对象中定义的属性。

## 创建对象

可以使用对象字面量、`new` 关键字和 ECMAScript 5中的 `Object.create()` 函数来创建对象。

### 使用对象字面量创建对象（推荐）

创建对象最简单的方式就是在 JavaScript 代码中使用对象字面量。对象字面量是由若干名值对组成的映射表，名值对中间用冒号分隔，名值对之间用逗号分隔，整个映射表用花括号括起来。属性名可以是 JavaScript 标识符也可以是字符串直接量（包括空字符串）。属性的值可以是任意类型的 JavaScript 表达式，表达式的值（可以是原始值也可以是对象值）就是这个属性的值。例如：

``` javascript
// 推荐写法
var person = {
    name : "stone",
    age : 28
};

// 也可以写成
var person = {};
person.name = "stone";
person.age = 28;
```

### 使用 `new` 关键字创建对象

new 关键字创建并初始化一个新对象。关键字 new 后跟随一个函数调用。这里的函数称做构造函数（constructor），构造函数用以初始化一个新创建的对象。JavaScript 语言核心中的原始类型都包含内置构造函数。例如：

``` javascript
var person = new Object();
person.name = "stone";
person.age = 28;
```

其中 `var person = new Object();` 等价于 `var person = {};` 。


### 使用 `Object.create()` 函数创建对象

ECMAScript 5定义了一个名为 `Object.create()` 的方法，它创建一个新对象，其中第一个参数是这个对象的原型。`Object.create()` 提供第二个可选参数，用以对对象的属性进行进一步描述。`Object.create()` 是一个静态函数，而不是提供给某个对象调用的方法。使用它的方法很简单，只须传入所需的原型对象即可。例如：

``` javascript
var person = Object.create(Object.prototype);
person.name = "stone";
person.age = 28;
```

其中 `var person = Object.create(Object.prototype);` 也等价于 `var person = {};` 。

### 原型（prototype）

所有通过对象字面量创建的对象都具有同一个原型对象，并可以通过 JavaScript 代码 `Object.prototype` 获得对原型对象的引用。通过关键字 `new` 和构造函数调用创建的对象的原型就是构造函数的 `prototype` 属性的值。因此，同使用 `{}` 创建对象一样，通过 `new Object()` 创建的对象也继承自 `Object.prototype`。同样，通过 `new Array()` 创建的对象的原型就是 `Array.prototype`，通过 `new Date()` 创建的对象的原型就是 `Date.prototype`。

没有原型的对象为数不多，`Object.prototype` 就是其中之一。它不继承任何属性。其他原型对象都是普通对象，普通对象都具有原型。所有的内置构造函数（以及大部分自定义的构造函数）都具有一个继承自 `Object.prototype` 的原型。例如，`Date.prototype` 的属性继承自 `Object.prototype`，因此由 `new Date()` 创建的 `Date` 对象的属性同时继承自 `Date.prototype` 和 `Object.prototype`。

这一系列链接的原型对象就是所谓的「原型链（prototype chain）」。

## 属性的查询和设置

前面有提到过，可以通过点 `.` 或方括号 `[]` 运算符来获取属性的值。对于点 `.` 来说，左侧应当是一个对象，右侧必须是一个以属性名称命名的简单标识符。对于方括号来说 `[]` ，方括号内必须是一个计算结果为字符串的表达式，这个字符串就是属性的名称。例如：

``` javascript
// 推荐写法
console.log(person.name);   // "stone"
console.log(person.age);    // "28"

// 也可以写成
console.log(person["name"]);    // stone
console.log(person["age"]);     // 28
```

和获取属性的值写法一样，通过点和方括号也可以创建属性或给属性赋值，但需要将它们放在赋值表达式的左侧。例如：

``` javascript
// 推荐写法
person.name = "sophie"; // 赋值
person.age = 30;        // 赋值
person.weight = 38;     // 创建

// 也可以写成
person["name"] = "sophie";  // 赋值
person["age"] = 30;         // 赋值
person["weight"] = 38;      // 创建
```

当使用方括号时，方括号内的表达式必须返回字符串。更严格地讲，表达式必须返回字符串或返回一个可以转换为字符串的值。

## 属性的访问错误

查询一个不存在的属性并不会报错，如果在对象 `o` 自身的属性或继承的属性中均未找到属性 `x`，属性访问表达式 `o.x` 返回 `undefined`。例如：

``` javascript
var person = {};
person.wife;    // undefined
```

但是，如果对象不存在，那么试图查询这个不存在的对象的属性就会报错。`null` 和 `undefined` 值都没有属性，因此查询这些值的属性会报错。例如：

``` javascript
var person = {};
person.wife.name;   // Uncaught TypeError: Cannot read property 'name' of undefined.
```

除非确定 `person` 和 `person.wife` 都是对象，否则不能这样写表达式 `person.wife.name`，因为会报「未捕获的错误类型」，下面提供了两种避免出错的方法：

``` javascript
// 冗余但易懂的写法
var name;
if (person) {
    if (person.wife) 
        name = person.wife.name;
}

// 简练又常用的写法（推荐写法）
var name = person && person.wife && person.wife.name;
```

## 删除属性

`delete` 运算符用来删除对象属性，事实上 `delete` 只是断开属性和宿主对象的联系，并没有真正的删除它。`delete` 运算符只能删除自有属性，不能删除继承属性（要删除继承属性必须从定义这个属性的原型对象上删除它，而且这会影响到所有继承自这个原型的对象）。

> 代码范例，请参见[「变量和数据类型」-「数据类型」-「delete 运算符」](https://github.com/stone0090/javascript-lessons/tree/master/1.5-Expression%26Operators#delete-运算符)。

## 检测属性

JavaScript 对象可以看做属性的集合，我们经常会检测集合中成员的所属关系（判断某个属性是否存在于某个对象中）。可以通过 `in` 运算符、`hasOwnPreperty()` 和 `propertyIsEnumerable()` 来完成这个工作，甚至仅通过属性查询也可以做到这一点。

`in` 运算符的左侧是属性名（字符串），右侧是对象。如果对象的自有属性或继承属性中包含这个属性则返回 `true`。例如：

``` javascript
var o = { x: 1 }
console.log("x" in o);          // true，x是o的属性
console.log("y" in o);          // false，y不是o的属性
console.log("toString" in o);   // true，toString是继承属性
```

对象的 `hasOwnProperty()` 方法用来检测给定的名字是否是对象的自有属性。对于继承属性它将返回 `false`。例如：

``` javascript
var o = { x: 1 }
console.log(o.hasOwnProperty("x"));          // true，x是o的自有属性
console.log(o.hasOwnProperty("y"));          // false，y不是o的属性
console.log(o.hasOwnProperty("toString"));   // false，toString是继承属性
```

`propertyIsEnumerable()` 是 `hasOwnProperty()` 的增强版，只有检测到是自有属性且这个属性的可枚举性（enumerable attribute）为 `true` 时它才返回 `true`。某些内置属性是不可枚举的。通常由 JavaScript 代码创建的属性都是可枚举的，除非在 ECMAScript 5中使用一个特殊的方法来改变属性的可枚举性。例如：

``` javascript
var o = inherit({ y: 2 });
o.x = 1;
o.propertyIsEnumerable("x");    // true:，x是o的自有属性，可枚举
o.propertyIsEnumerable("y");    // false，y是继承属性
Object.prototype.propertyIsEnumerable("toString");  // false，不可枚举
```

除了使用 `in` 运算符之外，另一种更简便的方法是使用 `!==` 判断一个属性是否是 `undefined`。例如：

``` javascript
var o = { x: 1 }
console.log(o.x !== undefined);              // true，x是o的属性
console.log(o.y !== undefined);              // false，y不是o的属性
console.log(o.toString !== undefined);       // true，toString是继承属性
```

然而有一种场景只能使用 `in` 运算符而不能使用上述属性访问的方式。`in` 可以区分不存在的属性和存在但值为 `undefined` 的属性。例如：

``` javascript
var o = { x: undefined }        // 属性被显式赋值为undefined
console.log(o.x !== undefined); // false，属性存在，但值为undefined
console.log(o.y !== undefined); // false，属性不存在
console.log("x" in o);          // true，属性存在
console.log("y" in o);          // false，属性不存在
console.log(delete o.x);        // true，删除了属性x
console.log("x" in o);          // false，属性不再存在
```

> 扩展阅读「JavaScript 检测原始值、引用值、属性」  
> http://shijiajie.com/2016/06/20/javascript-maintainable-javascript-validate1/

> 扩展阅读「JavaScript 检测之 basevalidate.js」  
> http://shijiajie.com/2016/06/25/javascript-maintainable-javascript-basevalidatejs/

## 枚举属性

除了检测对象的属性是否存在，我们还会经常遍历对象的属性。通常使用 `for-in` 循环遍历，ECMAScript 5提供了两个更好用的替代方案。

`for-in` 循环可以在循环体中遍历对象中所有可枚举的属性（包括自有属性和继承的属性），把属性名称赋值给循环变量。对象继承的内置方法不可枚举的，但在代码中给对象添加的属性都是可枚举的。例如：

``` javascript
var o = {x:1, y:2, z:3};            // 三个可枚举的自有属性
o.propertyIsEnumerable("toString"); // false，不可枚举
for (p in o) {          // 遍历属性
    console.log(p);     // 输出x、y和z，不会输出toString
}
```

有许多实用工具库给 `Object.prototype` 添加了新的方法或属性，这些方法和属性可以被所有对象继承并使用。然而在ECMAScript 5标准之前，这些新添加的方法是不能定义为不可枚举的，因此它们都可以在 `for-in` 循环中枚举出来。为了避免这种情况，需要过滤 `for-in` 循环返回的属性，下面两种方式是最常见的：

``` javascript
for(p in o) {
   if (!o.hasOwnProperty(p)) continue;          // 跳过继承的属性
   if (typeof o[p] === "function") continue;    // 跳过方法
}
```

除了 `for-in` 循环之外，ECMAScript 5定义了两个用以枚举属性名称的函数。第一个是 `Object.keys()`，它返回一个数组，这个数组由对象中可枚举的自有属性的名称组成。第二个是 `Object.getOwnPropertyNames()`，它和 `Ojbect.keys()` 类似，只是它返回对象的所有自有属性的名称，而不仅仅是可枚举的属性。在ECMAScript 3中是无法实现的类似的函数的，因为ECMAScript 3中没有提供任何方法来获取对象不可枚举的属性。

## 属性的 `getter` 和 `setter`

我们知道，对象属性是由名字、值和一组特性（attribute）构成的。在ECMAScript 5中，属性值可以用一个或两个方法替代，这两个方法就是 `getter` 和 `setter`。由 `getter` 和 `setter` 定义的属性称做「存取器属性（accessor property）」，它不同于「数据属性（data property）」，数据属性只有一个简单的值。

当程序查询存取器属性的值时，JavaScript 调用 `getter` 方法。这个方法的返回值就是属性存取表达式的值。当程序设置一个存取器属性的值时，JavaScript 调用 `setter` 方法，将赋值表达式右侧的值当做参数传入 `setter`。从某种意义上讲，这个方法负责「设置」属性值。可以忽略 `setter` 方法的返回值。

和数据属性不同，存取器属性不具有可写性（writable attribute）。如果属性同时具有 `getter` 和 `setter` 方法，那么它是一个读/写属性。如果它只有 `getter` 方法，那么它是一个只读属性。如果它只有 `setter` 方法，那么它是一个只写属性，读取只写属性总是返回 `undefined`。定义存取器属性最简单的方法是使用对象直接量语法的一种扩展写法。例如：

``` javascript
var o = {
    // 普通的数据属性
    data_prop: value,

    // 存取器属性都是成对定义的函数
    get accessor_prop() { /*这里是函数体 */ },
    set accessor_prop(value) { /* 这里是函数体*/ }
};
```

存取器属性定义为一个或两个和属性同名的函数，这个函数定义没有使用 `function` 关键字，而是使用 `get` 或 `set`。注意，这里没有使用冒号将属性名和函数体分隔开，但在函数体的结束和下一个方法或数据属性之间有逗号分隔。

## 序列化对象（JSON）

对象序列化（serialization）是指将对象的状态转换为字符串，也可将字符串还原为对象。ECMAScript 5提供了内置函数 `JSON.stringify()` 和 `JSON.parse()` 用来序列化和还原 JavaScript 对象。这些方法都使用 JSON 作为数据交换格式，JSON 的全称是「JavaScript 对象表示法（JavaScript Object Notation）」，它的语法和 JavaScript 对象与数组直接量的语法非常相近。例如：

``` javascript
o = {x:1, y:{z:[false,null,""]}};       // 定义一个对象
s = JSON.stringify(o);                  // s是 '{"x":1,"y":{"z":[false,null,""]}}'
p = JSON.parse(s);                      // p是o的深拷贝
```

ECMAScript 5中的这些函数的本地实现和 https://github.com/douglascrockford/JSON-js 中的公共域ECMAScript 3版本的实现非常类似，或者说完全一样，因此可以通过引入 `json2.js` 模块在ECMAScript 3的环境中使用ECMAScript 5中的这些函数。

JSON 的语法是 JavaScript 语法的子集，它并不能表示 JavaScript 里的所有值。它支持对象、数组、字符串、无穷大数字、`true`、`false` 和 `null`，可以序列化和还原它们。`NaN`、`Infinity` 和 `-Infinity` 序列化的结果是 `null`，日期对象序列化的结果是 ISO 格式的日期字符串（参照 `Date.toJSON()` 函数），但 `JSON.parse()` 依然保留它们的字符串形态，而不会将它们还原为原始日期对象。函数、`RegExp`、`Error` 对象和 `undefined` 值不能序列化和还原。`JSON.stringify()` 只能序列化对象可枚举的自有属性。对于一个不能序列化的属性来说，在序列化后的输出字符串中会将这个属性省略掉。`JSON.stringify()` 和 `JSON.parse()` 都可以接收第二个可选参数，通过传入需要序列化或还原的属性列表来定制自定义的序列化或还原操作。


## 关卡

请实现下面用来枚举属性的对象工具函数：

``` javascript
/*
 * 把 p 中的可枚举属性复制到 o 中，并返回 o
 * 如果 o 和 p 中含有同名属性，则覆盖 o 中的属性
 */
function extend(o, p) {
    // 请实现函数体
}
```

``` javascript
/*
 * 将 p 中的可枚举属性复制至 o 中，并返回 o
 * 如果 o 和 p 中有同名的属性，o 中的属性将不受影响
 */
function merge(o, p) {
    // 请实现函数体
}
```

``` javascript
/*
 * 如果 o 中的属性在 p 中没有同名属性，则从 o 中删除这个属性
 * 返回 o
 */
function restrict(o, p) {
    // 请实现函数体
}
```

``` javascript
/*
 * 如果 o 中的属性在 p 中存在同名属性，则从 o 中删除这个属性
 * 返回 o
 */
function subtract(o, p) {
    // 请实现函数体
}
```

``` javascript
/*
 * 返回一个新对象，这个对象同时拥有 o 的属性和 p 的属性
 * 如果 o 和 p 中有重名属性，使用 p 中的属性值
 */
function union(o, p) { 
    // 请实现函数体
}
```

``` javascript
/*
 * 返回一个新对象，这个对象拥有同时在 o 和 p 中出现的属性
 * 很像求 o 和 p 的交集，但 p 中属性的值被忽略
 */
function intersection(o, p) { 
    // 请实现函数体
}
```

``` javascript
/*
 * 返回一个数组，这个数组包含的是 o 中可枚举的自有属性的名字
 */
function keys(o) {
    // 请实现函数体
}
```

## 更多

> 关注微信公众号「劼哥舍」回复「答案」，可获取关卡详解。  
> 您还可以在 [GitHub](https://github.com/) 上 [Star](https://github.com/stone0090/javascript-lessons) 该课程，查看章节目录，获取最新内容。  
> https://github.com/stone0090/javascript-lessons

