# 数组

数组是值的有序集合。每个值叫做一个元素，而每个元素在数组中有一个位置，以数字表示，称为索引。

JavaScript 数组是无类型的，数组元素可以是任意类型，并且同一个数组中的不同元素也可能有不同的类型。数组的元素甚至也可能是对象或其他数组。

JavaScript 数组是动态的，根据需要它们会增长或缩减，并且在创建数组时无须声明一个固定的大小或者在数组大小变化时无须重新分配空间。

JavaScript 数组可能是稀疏的，数组元素的索引不一定要连续的，它们之间可以有空缺。每个 JavaScript 数组都有一个 `length` 属性。针对非稀疏数组，该属性就是数组元素的个数。针对稀疏数组，`length` 比所有元素的索引要大。

JavaScript 数组是 JavaScript 对象的特殊形式，数组索引实际上和碰巧是整数的属性名差不多。通常，数组的实现是经过优化的，用数字索引来访问数组元素一般来说比访问常规的对象属性要快很多。

数组继承自 `Array.prototype` 中的属性，它定义了一套丰富的数组操作方法。

## 创建数组

可以使用数组字面量和 `new` 关键字来创建数组。

### 使用数组字面量创建数组（推荐）

``` javascript
var empty = [];                 // 没有元素的数组
var primes = [2, 3, 5, 7, 11];  // 有5个数值的数组
var misc = [1.1, true, "a"];    // 3个不同类型的元素

// 数组直接量中的值不一定要是常量，可以是任意的表达式
var base = 1024;
var table = [base, base+1, base+2, base+3];

// 也可以包含对象直接量或其他数组直接量
var b = [[1, {x:1, y:2}], [2, {x:3, y:4}]];
```

注意，不要忽略数组字面量的最后一个元素，仅以逗号结尾。下面几个案例，在不同的浏览器下，可能会被识别成2个元素，也有可能识别成3个元素，而造成程序bug。例如：

``` javascript
var nums = [,,,];               // 不好的写法
var names = ["stone",,];        // 不好的写法
var colors = ["red","green",];  // 不好的写法
```

### 使用 `new` 关键字创建数组

使用 `new` 关键字调用构造函数 `Array()` 是创建数组的另一种方法，可以用三种方式调用构造函数。例如：

``` javascript
// 调用时没有参数
var a = new Array();

// 调用时有一个数值参数，它指定长度
var a = new Array(10); 

// 显式指定多个数组元素或者数组的一个非数值元素
var a = new Array(5, 4, 3, 2, 1, "testing");
```

## 数组元素的读和写

使用 `[]` 操作符来访问数组中的一个元素。数组的引用位于方括号的左边。方括号中是一个返回非负整数值的任意表达式。使用该语法既可以读又可以写数组的一个元素。例如：

``` javascript
var a = ["world"];     // 从一个元素的数组开始
var value = a[0];      // 读第0个元素
a[1] = 3.14;           // 写第1个元素
var i = 2; 
a[i] = 3;              // 写第2个元素
a[i + 1] = "hello";    // 写第3个元素
a[a[i]] = a[0];        // 读第0个和第2个元素，写第3个元素
```

请记住，数组是对象的特殊形式，可以为其创建任意名字的属性。但如果使用的属性是数组的索引，数组的特殊行为就是将根据需要更新它们的length属性值。

注意，可以使用负数或非整数来索引数组。这种情况下，数值转换为字符串，字符串作为属性名来用。既然名字不是非负整数，它就只能当做常规的对象属性，而非数组的索引。同样，如果凑巧使用了是非负整数的字符串，它就当做数组索引，而非对象属性。当使用的一个浮点数和一个整数相等时情况也是一样的。例如：

``` javascript
a[-1.23] = true;  // 这将创建一个名为"-1.23"的属性
a["1000"] = 0;    // 这是数组的第1001个元素
a[1.000]          // 和 a[1] 相等
```

事实上数组索引仅仅是对象属性名的一种特殊类型，这意味着 JavaScript 数组没有「越界」错误的概念。当试图查询任何对象中不存在的属性时，不会报错，只会得到 `undefined` 值。

## 稀疏数组

稀疏数组就是包含从0开始的不连续索引的数组。通常，数组的 `length` 属性值代表数组中元素的个数。如果数组是稀疏的，`length` 属性值大于元素的个数。可以用 `Array()` 构造函数或简单地指定数组的索引值大于当前的数组长度来创建稀疏数组。

``` javascript
a = new Array(5);   // 数组没有元素，但是 a.length = 5
a = [];             // 创建一个空数组，a.length = 0
a[1000] = 0;        // 添加一个元素，a.length 被自动更新为1001
```
足够稀疏的数组通常在实现上比稠密的数组更慢、内存利用率更高，在这样的数组中查找元素的时间与常规对象属性的查找时间一样长。

需要注意的是，当省略数组直接量中的值时（使用连续的逗号，比如 `[1,,3]` ），这时所得到的数组也是稀疏数组，省略掉的值是不存在的：

``` javascript
var a1 = [,'1','2'];    // 此数组长度是3 
var a2 = [undefined];   // 此数组包含一个值为 undefined 的元素 
console.log(0 in a1);   // false，a1 在索引0处没有元素
console.log(0 in a2);   // true，a2 在索引0处有一个值为 undefined 的元素 
```

了解稀疏数组是了解 JavaScript 数组的真实本质的一部分。尽管如此，实际上你所碰到的绝大多数 JavaScript 数组不是稀疏数组。并且，如果你确实碰到了稀疏数组，你的代码很可能像对待非稀疏数组一样来对待它们，只不过它们包含一些 `undefined` 值。

## 数组长度

每个数组有一个 `length` 属性，就是这个属性使其区别于常规的 JavaScript 对象。针对稠密（也就是非稀疏）数组，`length` 属性值代表数组中元素的个数。其值比数组中最大的索引大1。例如：

``` javascript
[].length             // 0，数组没有元素
['a','b','c'].length  // 3，最大的索引为2，length 为3
```

当数组是稀疏的时，`length` 属性值大于元素的个数。而且关于此我们可以说的一切也就是数组长度保证大于它每个元素的索引值。或者，换一种说法，在数组中（无论稀疏与否）肯定找不到一个元素的索引值大于或等于它的长度。为了维持此规则不变化，数组有两个特殊的行为。

- 第一个如同上面的描述：如果为一个数组元素赋值，它的索引 `i` 大于或等于现有数组的长度时，`length` 属性的值将设置为 `i+1`。
- 第二个特殊的行为就是设置 `length` 属性为一个小于当前长度的非负整数 `n` 时，当前数组中那些索引值大于或等于 `n` 的元素将从中删除。例如：

``` javascript
a = [1,2,3,4,5];     // 从5个元素的数组开始
a.length = 3;        // 现在 a 为[1,2,3]
a.length = 0;        // 删除所有的元素。a 为[ ]
a.length = 5;        // 长度为5，但是没有元素，就像 new Array(5)
```

还可以将数组的 `length` 属性值设置为大于其当前的长度。实际上这不会向数组中添加新的元素，它只是在数组尾部创建一个空的区域。

在 ECMAScript 5 中，可以用 `Object.defineProperty()` 让数组的 `length` 属性变成只读的。例如：

``` javascript
a = [1,2,3];                                            // 从3个元素的数组开始
Object.defineProperty(a, "length", {writable: false});  // 让 length 属性只读
a.length = 0;                                           // a 不会改变
```

## 数组元素的添加和删除

我们已经见过添加数组元素最简单的方法，为新索引赋值。例如：

``` javascript
a = []           // 开始是一个空数组
a[0] = "zero";   // 然后向其中添加元素
a[1] = "one";
```

也可以使用 `push()` 方法在数组末尾增加一个或多个元素。例如：

``` javascript
a = [];                 // 开始是一个空数组
a.push("zero");         // 在末尾添加一个元素。a = ["zero"]
a.push("one", "two");   // 再添加两个元素。a = ["zero", "one", "two"]
```

可以像删除对象属性一样使用 `delete` 运算符来删除数组元素。例如：

``` javascript
a = [1,2,3]; 
delete a[1];   // a在索引1的位置不再有元素
1 in a         // => false: 数组索引1并未在数组中定义
a.length       // => 3: delete操作并不影响数组长度
```

注意，对一个数组元素使用 `delete` 不会修改数组的 `length` 属性，也不会将元素从高索引处移下来填充已删除属性留下的空白。如果从数组中删除一个元素，它就变成稀疏数组。

## 数组遍历

使用 `for` 循环是遍历数组元素最常见的方法。例如：

``` javascript
var keys = Object.keys(o);   // 获得 o 对象属性名组成的数组
var values = []              // 在数组中存储匹配属性的值
for(var i = 0; i < keys.length; i++) {  // 对于数组中每个索引
    var key = keys[i];                  // 获得索引处的键值
    values[i] = o[key];                 // 在 values 数组中保存属性值
}
```

在嵌套循环或其他性能非常重要的上下文中，可以看到这种基本的数组遍历需要优化，数组的长度应该只查询一次而非每次循环都要查询。例如：

``` javascript
for(var i = 0, len = keys.length; i < len; i++) {
   // 循环体仍然不变
}
```

这些例子假设数组是稠密的，并且所有的元素都是合法数据。否则，使用数组元素之前应该先检测它们。例如：

``` javascript
for(var i = 0; i < a.length; i++) {
    if (!a[i]) continue;    // 跳过 null、undefined 和不存在的元素
    if (!(i in a)) continue ;   // 跳过不存在的元素
    if (a[i] === undefined) continue;   // 跳过 undefined 和不存在的元素
    // 循环体
}
```

还可以使用 `for-in` 循环处理稀疏数组。循环每次将一个可枚举的属性名（包括数组索引）赋值给循环变量，不存在的索引将不会遍历到。例如：

``` javascript
for(var index in sparseArray) {
   var value = sparseArray[index];
   // 此处可以使用索引和值做一些事情
}
```

但由于 `for-in` 循环能够枚举继承的属性名，如添加到 `Array.prototype` 中的方法。基于这个原因，在数组上不应该使用 `for-in` 循环，除非使用额外的检测方法来过滤不想要的属性。例如：

``` javascript
for(var i in a) {
    // 跳过继承的属性
    if (!a.hasOwnProperty(i)) continue;

    // 跳过不是非负整数的 i
    if (String(Math.floor(Math.abs(Number(i)))) !== i) continue;
}
```

`JavaScript` 规范允许 `for-in` 循环以不同的顺序遍历对象的属性。通常数组元素的遍历实现是升序的，但不能保证一定是这样的。如果数组同时拥有对象属性和数组元素，返回的属性名很可能是按照创建的顺序而非数值的大小顺序。如何处理这个问题的实现，各个浏览器都不相同，如果算法依赖于遍历的顺序，那么最好不要使用 `for-in` 而用常规的 `for` 循环。

ECMAScript 5 定义了一些遍历数组元素的新方法，按照索引的顺序按个传递给定义的一个函数。这些方法中最常用的就是 `forEach()` 方法。例如：

``` javascript
var data = [1,2,3,4,5];     // 这是需要遍历的数组
var sumOfSquares = 0;       // 要得到数据的平方和
data.forEach(function(x) {  // 把每个元素传递给此函数
    sumOfSquares += x*x;    // 平方相加
});
console.log(sumOfSquares);  // 55，1 + 4 + 9 + 16 + 25
```

## 数组检测

给定一个未知的对象，判定它是否为数组通常非常有用。在 ECMAScript 5 中，可以使用 `Array.isArray()` 函数来做这件事情。例如：

``` javascript
Array.isArray([])   // true
Array.isArray({})   // false
```

但是，在 ECMAScript 5 以前，要区分数组和非数组对象很困难。`typeof` 运算符对数组返回 `"object"`（并且对于除了函数以外的所有对象都是如此）。`instanceof` 操作符也只能用于简单的情形。例如：

``` javascript
[] instanceof Array     // true
({}) instanceof Array   // false
```

使用 `instanceof` 的问题是在 Web 浏览器中有可能有多个窗体存在。每个窗体都有自己的 JavaScript 环境，有自己的全局对象。并且，每个全局对象有自己的一组构造函数。因此一个窗体中的对象将不可能是另外窗体中的构造函数的实例。窗体之间的混淆不常发生，但这个问题足已证明 `instanceof` 操作符不能视为一个可靠的数组检测方法。

解决方案是检查对象的类属性，对数组而言该属性的值总是 `"Array"`，因此在 ECMAScript 3 中 `isArray()` 函数的代码可以这样书写。例如：

``` javascript
var isArray = Array.isArray || function(o) {
    return typeof o === "object" && Object.prototype.toString.call(o) === "[object Array]";
};
```

## 数组方法

ECMAScript 3 和 ECMAScript 5 在 `Array.prototype` 中定义了一些很有用的操作数组的方法。

### 转换方法

所有对象都具有 `toLocaleString()`、`toString()` 和 `valueOf()` 方法。其中，调用数组的 `toString()` 和 `valueOf()` 方法会返回相同的值，即由数组中每个值的字符串形式拼接而成的一个以逗号分隔的字符串。实际上，为了创建这个字符串会调用数组每一项的 `toString()` 方法。例如：

``` javascript
var colors = ["red", "blue", "green"];  // 创建一个包含3个字符串的数组
alert(colors.toString()); // red,blue,green
alert(colors.valueOf());  // red,blue,green
alert(colors);            // red,blue,green
```

在这里，我们首先显式地调用了 `toString()` 和 `valueOf()` 方法，以便返回数组的字符串表示，每个值的字符串表示拼接成了一个字符串，中间以逗号分隔。最后一行代码直接将数组传递给了 `alert()`。由于 `alert()`要接收字符串参数，所以它会在后台调用 `toString()` 方法，由此会得到与直接调用 `toString()` 方法相同的结果。

另外，`toLocaleString()` 方法经常也会返回与 `toString()` 和 `valueOf()` 方法相同的值，但也不总是如此。当调用数组的 `toLocaleString()` 方法时，它也会创建一个数组值的以逗号分隔的字符串。而与前两个方法唯一的不同之处在于，这一次为了取得每一项的值，调用的是每一项的 `toLocaleString()` 方法，而不是 `toString()` 方法。例如：

``` javascript
var person1 = {
    toLocaleString : function () {
        return "Nikolaos";
    },
    toString : function() {
        return "Nicholas";
    }
};

var person2 = {
    toLocaleString : function () {
        return "Grigorios";
    },
    toString : function() {
        return "Greg";
    }
};

var people = [person1, person2];
alert(people);                           // Nicholas,Greg
alert(people.toString());                // Nicholas,Greg
alert(people.toLocaleString());          // Nikolaos,Grigorios
```

数组继承的 `toLocaleString()`、`toString()` 和 `valueOf()`方法，在默认情况下都会以逗号分隔的字符串的形式返回数组项。而如果使用 `join()` 方法，则可以使用不同的分隔符来构建这个字符串。`join()` 方法只接收一个参数，即用作分隔符的字符串，然后返回包含所有数组项的字符串。例如：

``` javascript
var colors = ["red", "green", "blue"];
console.log(colors.join(","));    // red,green,blue
console.log(colors.join("||"));   // red||green||blue
```

 如果数组中的某一项的值是 `null` 或者 `undefined`，那么该值在 `join()`、`toLocaleString()`、`toString()` 和 `valueOf()` 方法返回的结果中以空字符串表示。

### 栈方法

栈是一种 LIFO（Last-In-First-Out，后进先出）的数据结构，也就是最新添加的项最早被移除。`push()` 方法可以接收任意数量的参数，把它们逐个添加到数组末尾，并返回修改后数组的长度。而 `pop()` 方法则从数组末尾移除最后一项，减少数组的 `length` 值，然后返回移除的项。结合 `push()` 和 `pop()` 方法，就可以像栈一样使用数组。例如：

``` javascript
var colors = [];                            // 创建一个数组
var count = colors.push("red", "green");    // 推入两项
console.log(count);                         // 2，数组的长度

count = colors.push("black");               // 推入另一项
console.log(count);                         // 3，数组的长度

var item = colors.pop();                    // 取得最后一项
console.log(item);                          // "black"
console.log(colors.length);                 // 2，数组的长度
```

### 队列方法

队列是一种 FIFO（First-In-First-Out，先进先出）的数据结构，队列在列表的末端添加项，从列表的前端移除项。`shift()` 方法则从数组前端移除第一项，减少数组的 `length` 值，然后返回移除的项。结合 `push()` 和 `shift()` 方法，就可以像队列一样使用数组。例如：

``` javascript
var colors = [];                            // 创建一个数组
var count = colors.push("red", "green");    // 推入两项
console.log(count);                         // 2，数组的长度

count = colors.push("black");               // 推入另一项
console.log(count);                         // 3，数组的长度

var item = colors.shift();                  // 取得第一项
console.log(item);                          // "red"
console.log(colors.length);                 // 2，数组的长度
```

JavaScipt 还为数组提供了一个 `unshift()` 方法。顾名思义，`unshift()` 与 `shift()` 的用途相反，它能在数组前端添加任意个项并返回新数组的长度。因此，同时使用 `unshift()` 和 `pop()` 方法，可以从相反的方向来模拟队列，即在数组的前端添加项，从数组末端移除项。

### 重排序方法

数组中有两个重排序的方法：`reverse()` 和 `sort()`。`reverse()` 方法可以反转数组元素的顺序。例如：

``` javascript
var values = [1, 2, 3, 4, 5];
values.reverse();
console.log(values);  // 5,4,3,2,1
```

`sort()` 方法可以按升序排列数组元素（即最小的值位于最前面，最大的值排在最后面）。`sort()` 方法在排序的过程中会调用每个数组元素的 `toString()`，然后比较得到的字符串，以确定如何排序。即使数组中的每一项都是数值，`sort()` 方法比较的也是字符串，例如：

``` javascript
var values = [0, 1, 5, 10, 15];
values.sort();
console.log(values);     // 0,1,10,15,5
```

这种排序方式在很多情况下都不是最佳方案，因此 `sort()` 方法可以接收一个比较函数作为参数，以便我们指定哪个值位于哪个值的前面，以下就是一个简单的比较函数。例如：



``` javascript
function compare(value1, value2) {
    if (value1 < value2) {
        return -1;
    } else if (value1 > value2) {
        return 1;
    } else {
        return 0;
    }
}
```

这个比较函数接收两个参数，如果第一个参数应该位于第二个之前则返回一个负数，如果两个参数相等则返回0，如果第一个参数应该位于第二个之后则返回一个正数。它可以适用于大多数情况，只要将其作为参数传递给 `sort()` 方法即可。例如：

``` javascript
var values = [10, 5, 1, 0, 15];
values.sort(compare);
console.log(values);   // 0,1,5,10,15
```

对于数值类型或者其 `valueOf()` 方法会返回数值类型的对象类型，可以使用一个更简单的比较函数。这个函数只要用第二个值减第一个值即可。例如：

``` javascript
function compare(value1, value2){
    return value2 - value1;
}
```

由于比较函数通过返回一个小于零、等于零或大于零的值来影响排序结果，因此减法操作就可以适当地处理所有这些情况。

### 操作方法

JavaScript 为操作已经包含在数组中的元素提供了很多方法。其中，`concat()` 方法可以基于当前数组中的所有项创建一个新数组。具体来说，这个方法会先创建当前数组一个副本，然后将接收到的参数添加到这个副本的末尾，最后返回新构建的数组。在没有给 `concat()` 方法传递参数的情况下，它只是复制当前数组并返回副本。如果传递给 `concat()` 方法的是一或多个数组，则该方法会将这些数组中的每一项都添加到结果数组中。如果传递的值不是数组，这些值就会被简单地添加到结果数组的末尾。例如：

``` javascript
var colors = ["red", "green", "blue"];
var colors2 = colors.concat("yellow", ["black", "brown"]);

console.log(colors);     // red,green,blue
console.log(colors2);    // red,green,blue,yellow,black,brown
```

下一个方法是 `slice()`，它能够基于当前数组中的一或多个项创建一个新数组。`slice()` 方法可以接受一或两个参数，即要返回项的起始和结束位置。在只有一个参数的情况下，`slice()` 方法返回从该参数指定位置开始到当前数组末尾的所有项。如果有两个参数，该方法返回起始和结束位置之间的项，但不包括结束位置的项。注意，`slice()` 方法不会影响原始数组。例如：

``` javascript
var colors = ["red", "green", "blue", "yellow", "purple"];
var colors2 = colors.slice(1);
var colors3 = colors.slice(1,4);

console.log(colors2);   // green,blue,yellow,purple
console.log(colors3);   // green,blue,yellow
```

> 如果 `slice()` 方法的参数中有一个负数，则用数组长度加上该数来确定相应的位置。例如，在一个包含5项的数组上调用 `slice(-2,-1)` 与调用 `slice(3,4)` 得到的结果相同。如果结束位置小于起始位置，则返回空数组。

下一个方法是 `splice()`，它的主要用途是向数组的中部插入元素，主要有以下3种使用方式。

- 删除：可以删除任意数量的项，只需指定2个参数：起始位置和要删除元素的数量。例如，`splice(0,2)` 会删除数组中的前两项。
- 插入：可以向指定位置插入任意数量的项，只需提供3个参数：起始位置、0（要删除元素的数量）和要插入的元素。如果要插入多个元素，可以再传入第四、第五，以至任意多个元素。例如，`splice(2,0,"red","green")` 会从当前数组的位置2开始插入字符串 `"red"` 和 `"green"`。
- 替换：可以向指定位置插入任意数量的项，且同时删除任意数量的项，只需指定3个参数：起始位置、要删除元素的数量和要插入的元素。插入的项数不必与删除的项数相等。例如，`splice (2,1,"red","green")`会删除当前数组位置2的项，然后再从位置2开始插入字符串 `"red"` 和 `"green"`。

`splice()` 方法始终都会返回一个数组，该数组中包含从原始数组中删除的项（如果没有删除任何项，则返回一个空数组）。下面的代码展示了上述3种使用 `splice()` 方法的方式。例如：

``` javascript
var colors = ["red", "green", "blue"];
var removed = colors.splice(0,1);       // 删除第一项
console.log(colors);                    // green,blue
console.log(removed);                   // red，返回的数组中只包含一项

removed = colors.splice(1, 0, "yellow", "orange");  // 从位置1开始插入两项
console.log(colors);                    // green,yellow,orange,blue
console.log(removed);                   // 返回的是一个空数组

removed = colors.splice(1, 1, "red", "purple");     // 插入两项，删除一项
console.log(colors);                    // green,red,purple,orange,blue
console.log(removed);                   // yellow，返回的数组中只包含一项
```

### 位置方法

ECMAScript 5 为数组实例添加了两个位置方法：`indexOf()` 和 `lastIndexOf()`。这两个方法都接收两个参数：要查找的项和（可选的）表示查找起点位置的索引。其中，`indexOf()` 方法从数组的开头（位置0）开始向后查找，`lastIndexOf()` 方法则从数组的末尾开始向前查找。

这两个方法都返回要查找的项在数组中的位置，或者在没找到的情况下返回 `-1`。在比较第一个参数与数组中的每一项时，会使用全等操作符；也就是说，要求查找的项必须严格相等（就像使用 `===` 一样）。例如：

``` javascript
var numbers = [1,2,3,4,5,4,3,2,1];
console.log(numbers.indexOf(4));          // 3
console.log(numbers.lastIndexOf(4));      // 5
console.log(numbers.indexOf(4, 4));       // 5
console.log(numbers.lastIndexOf(4, 4));   // 3

var person = { name: "Nicholas" };
var people = [{ name: "Nicholas" }];
var morePeople = [person];
console.log(people.indexOf(person));      // -1
console.log(morePeople.indexOf(person));  // 0
```

### 迭代方法

ECMAScript 5 为数组定义了5个迭代方法。每个方法都接收两个参数：要在每一项上运行的函数和（可选的）运行该函数的作用域对象。传入这些方法中的函数会接收三个参数：数组项的值、该项在数组中的位置和数组对象本身。根据使用的方法不同，这个函数执行后的返回值可能会也可能不会影响访问的返回值。以下是这5个迭代方法的作用。

- `every()`，对数组中的每一项运行给定函数，如果该函数对每一项都返回 `true` ，则返回 `true`。
- `filter()`，对数组中的每一项运行给定函数，返回该函数会返回 `true` 的项组成的数组。
- `forEach()`，对数组中的每一项运行给定函数。这个方法没有返回值。
- `map()`，对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。
- `some()`，对数组中的每一项运行给定函数，如果该函数对任一项返回 `true` ，则返回 `true`。

以上方法都不会修改数组中的包含的值。在这些方法中，最相似的是 `every()` 和 `some()`，它们都用于查询数组中的项是否满足某个条件。对 `every()` 来说，传入的函数必须对每一项都返回 `true`，这个方法才返回 `true`；否则，它就返回 `false`。而 `some()`方法则是只要传入的函数对数组中的某一项返回 `true`，就会返回 `true`。例如：

``` javascript
var numbers = [1,2,3,4,5,4,3,2,1];
var everyResult = numbers.every(function(item, index, array){
    return (item > 2); 
});
console.log(everyResult);   // false

var someResult = numbers.some(function(item, index, array){
    return (item > 2);
});
console.log(someResult);    // true
```

下面再看一看 `filter()` 函数，它利用指定的函数确定是否在返回的数组中包含的某一项。例如：

``` javascript
var numbers = [1,2,3,4,5,4,3,2,1];
var filterResult = numbers.filter(function(item, index, array){
    return (item > 2);
});
console.log(filterResult);  // [3,4,5,4,3]
```

`map()` 也返回一个数组，而这个数组的每一项都是在原始数组中的对应项上运行传入函数的结果。例如，可以给数组中的每一项乘以2，然后返回这些乘积组成的数组。例如：

``` javascript
var numbers = [1,2,3,4,5,4,3,2,1];
var mapResult = numbers.map(function(item, index, array){
    return item * 2;
});
console.log(mapResult);     // [2,4,6,8,10,8,6,4,2]
```

最后一个方法是 `forEach()`，它只是对数组中的每一项运行传入的函数。这个方法没有返回值，本质上与使用 `for` 循环迭代数组一样。例如：

``` javascript
var numbers = [1,2,3,4,5,4,3,2,1];
numbers.forEach(function(item, index, array){
    //执行某些操作 
});
```

###　缩小方法

ECMAScript 5 还新增了两个缩小数组的方法：`reduce()` 和 `reduceRight()`。这两个方法都会迭代数组的所有项，然后构建一个最终返回的值。其中，`reduce()` 方法从数组的第一项开始，逐个遍历到最后。而 `reduceRight()` 则从数组的最后一项开始，向前遍历到第一项。

这两个方法都接收两个参数：一个在每一项上调用的函数和（可选的）作为缩小基础的初始值。传给 `reduce()` 和` reduceRight()` 的函数接收4个参数：前一个值、当前值、项的索引和数组对象。这个函数返回的任何值都会作为第一个参数自动传给下一项。第一次迭代发生在数组的第二项上，因此第一个参数是数组的第一项，第二个参数就是数组的第二项。

使用 `reduce()` 方法可以执行求数组中所有值之和的操作。例如：

``` javascript
var values = [1,2,3,4,5];
var sum = values.reduce(function(prev, cur, index, array){
    return prev + cur; 
});
console.log(sum); // 15
```

第一次执行回调函数，prev是1，cur是2。第二次，prev是3（1加2的结果），cur是3（数组的第三项）。这个过程会持续到把数组中的每一项都访问一遍，最后返回结果。

`reduceRight()` 的作用类似，只不过方向相反而已。例如：

``` javascript
var values = [1,2,3,4,5];
var sum = values.reduceRight(function(prev, cur, index, array){
    return prev + cur;
});
console.log(sum); // 15
```

使用 `reduce()` 还是 `reduceRight()`，主要取决于要从哪头开始遍历数组。除此之外，它们完全相同。

## 关卡

完成下面3个数组去重方法。

```javascript
// 挑战一，一维数组
var arr = [2,3,4,2,3,5,6,4,3,2];
var unique = function(arr){
    // 待实现方法体
}
console.log(unique(arr)); // [2,3,4,5,6]
```

```javascript
// 挑战二，二维数组
var arr = [2,3,4,[2,3,4,5],3,5,[2,3,4,2],4,3,6,2];
var unique = function(arr){
    // 待实现方法体
}
console.log(unique(arr)); // [2,3,4,5,6]
```

```javascript
// 挑战三，三维数组或 n 维数组
var arr = [2,3,4,[2,3,[2,3,4,2],5],3,5,[2,3,[2,3,4,2],2],4,3,6,2];
var unique = function(arr){
    // 待实现方法体
}
console.log(unique(arr)); // [2,3,4,5,6]
```

## 更多

> 关注微信公众号「劼哥舍」回复「答案」，获取关卡详解。  
> 关注 [https://github.com/stone0090/javascript-lessons](https://github.com/stone0090/javascript-lessons)，获取最新动态。

