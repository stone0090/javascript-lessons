# 函数

函数是一段代码，它只定义一次，但可以被执行或调用任意次。在 JavaScript 里，函数即对象，程序可以随意操控它们。比如，可以把函数赋值给变量，或者作为参数传递给其他函数，也可以给它们设置属性，甚至调用它们的方法。如果函数挂载在一个对象上，作为对象的一个属性，就称它为对象的方法。如果函数嵌套在其他函数中定义，这样它们就可以访问它们被定义时所处的作用域中的任何变量。

## 函数定义

在 JavaScript 中，函数实际上是对象，每个函数都是 `Function` 构造函数的实例，因此函数名实际上也是一个指向函数对象的指针，不会与某个函数绑定。函数通常有以下3中定义方式。例如：

``` javascript
// 写法一：函数声明（推荐写法）
function sum (num1, num2) {
    return num1 + num2;
}

// 写法二：函数表达式（推荐写法）
var sum = function(num1, num2){
    return num1 + num2;
};

// 写法三：Function 构造函数（不推荐写法）
var sum = new Function("num1", "num2", "return num1 + num2"); 
```

由于函数名仅仅是指向函数的指针，因此函数名与包含对象指针的其他变量没有什么不同。换句话说，一个函数可能会有多个名字。例如：

``` javascript
function sum(num1, num2){
    return num1 + num2;
}
console.log(sum(10,10));        // 20

var anotherSum = sum;
console.log(anotherSum(10,10)); // 20

sum = null;
console.log(anotherSum(10,10)); // 20
```

## 没有重载

将函数名想象为指针，也有助于理解为什么 JavaScript 中没有函数重载的概念。

``` javascript
function addSomeNumber(num){
    return num + 100;
}

function addSomeNumber(num) {
    return num + 200;
}

var result = addSomeNumber(100);    // 300
```

显然，这个例子中声明了两个同名函数，而结果则是后面的函数覆盖了前面的函数。以上代码实际上与下面的代码没有什么区别。

``` javascript
var addSomeNumber = function (num){
    return num + 100;
};

addSomeNumber = function (num) {
    return num + 200;
};

var result = addSomeNumber(100);    // 300
```

通过重写代码之后可以很容易明白，在创建第二个函数时，实际上覆盖了引用第一个函数的变量 `addSomeNumber`。

## 函数声明与函数表达式

解析器在向执行环境中加载数据时，对「函数声明」和「函数表达式」并非一视同仁。解析器会率先读取函数声明，并使其在执行任何代码之前可用（可以访问）；至于函数表达式，则必须等到解析器执行到它所在的代码行，才会真正被解释执行。例如：

``` javascript
console.log(sum(10,10)); // 20
function sum(num1, num2){
    return num1 + num2;
}
```

以上代码完全可以正常运行。因为在代码开始执行之前，解析器就已经通过一个名为函数声明提升（function declaration hoisting）的过程，读取并将函数声明添加到执行环境中。对代码求值时，JavaScript 引擎在第一遍会声明函数并将它们放到源代码树的顶部。所以，即使声明函数的代码在调用它的代码后面，JavaScript 引擎也能把函数声明提升到顶部。把上面的「函数声明」改为等价的「函数表达式」，就会在执行期间导致错误。例如：

``` javascript
console.log(sum(10,10)); // Uncaught TypeError: sum is not a function
var sum = function(num1, num2){
    return num1 + num2;
};
```

除了上述区别之外，「函数声明」与「函数表达式」的语法是等价的。

## 作为值的函数

因为 JavaScript 中的函数名本身就是变量，所以函数也可以作为值来使用。也就是说，不仅可以像传递参数一样把一个函数传递给另一个函数，而且可以将一个函数作为另一个函数的结果返回。来看一看下面的函数。

``` javascript
function callSomeFunction(someFunction, someArgument){
    return someFunction(someArgument);
}
```

这个函数接受两个参数。第一个参数应该是一个函数，第二个参数应该是要传递给该函数的一个值。然后，就可以像下面的例子一样传递函数了。

``` javascript
function add10(num){
    return num + 10;
}

var result1 = callSomeFunction(add10, 10);
console.log(result1);   // 20

function getGreeting(name){
    return "Hello, " + name;
}

var result2 = callSomeFunction(getGreeting, "Nicholas");
console.log(result2);   // "Hello, Nicholas"
```

这里的 `callSomeFunction()` 函数是通用的，即无论第一个参数中传递进来的是什么函数，它都会返回执行第一个参数后的结果。要访问函数的指针而不执行函数的话，必须去掉函数名后面的那对圆括号。因此上面例子中传递给 `callSomeFunction()` 的是 `add10` 和 `getGreeting`，而不是执行它们之后的结果。

当然，还可以从一个函数中返回另一个函数，而且这也是极为有用的一种技术。例如，假设有一个对象数组，我们想要根据某个对象属性对数组进行排序。而传递给数组 `sort()` 方法的比较函数要接收两个参数，即要比较的值。可是，我们需要一种方式来指明按照哪个属性来排序。要解决这个问题，可以定义一个函数，它接收一个属性名，然后根据这个属性名来创建一个比较函数，下面就是这个函数的定义。

``` javascript
function createComparisonFunction(propertyName) {
    return function(object1, object2){
        var value1 = object1[propertyName];
        var value2 = object2[propertyName];
        if (value1 < value2){
            return -1;
        } else if (value1 > value2){
            return 1;
        } else {
            return 0;
        }
    };
}
```

这个函数定义看起来有点复杂，但实际上无非就是在一个函数中嵌套了另一个函数，而且内部函数前面加了一个 `return` 操作符。在内部函数接收到 `propertyName` 参数后，它会使用方括号表示法来取得给定属性的值。取得了想要的属性值之后，定义比较函数就非常简单了。上面这个函数可以像在下面例子中这样使用。

``` javascript
var data = [{name: "Zachary", age: 28}, {name: "Nicholas", age: 29}];

data.sort(createComparisonFunction("name"));
console.log(data[0].name);  // Nicholas

data.sort(createComparisonFunction("age"));
console.log(data[0].name);  // Zachary
```

这里，我们创建了一个包含两个对象的数组 `data`。其中，每个对象都包含一个 `name` 属性和一个 `age` 属性。在默认情况下，`sort()` 方法会调用每个对象的 `toString()` 方法以确定它们的次序；但得到的结果往往并不符合人类的思维习惯。因此，我们调用 `createComparisonFunction("name")` 方法创建了一个比较函数，以便按照每个对象的 `name` 属性值进行排序。而结果排在前面的第一项是 `name` 为 `"Nicholas"`，`age` 是 `29` 的对象。然后，我们又使用了 `createComparisonFunction("age")` 返回的比较函数，这次是按照对象的age属性排序。得到的结果是 `name` 值为 `"Zachary"`，`age` 值是 `28` 的对象排在了第一位。

## 函数的形参和实参

在函数内部，有两个特殊的对象：`arguments` 和 `this`。其中，`arguments` 是一个类数组对象，包含着传入函数中的所有参数。虽然 `arguments` 的主要用途是保存函数参数，但这个对象还有一个名叫 `callee` 的属性，该属性是一个指针，指向拥有这个 `arguments` 对象的函数。请看下面这个非常经典的阶乘函数。

``` javascript
function factorial(num){
	if (num <= 1) {
        return 1;
    } else {
        return num * factorial(num-1)
    }
}
```

定义阶乘函数一般都要用到递归算法，如上面的代码所示，在函数有名字，而且名字以后也不会变的情况下，这样定义没有问题。但问题是这个函数的执行与函数名 `factorial` 紧紧耦合在了一起。为了消除这种紧密耦合的现象，可以像下面这样使用 `arguments.callee`。

``` javascript
function factorial(num){
    if (num <=1) {
        return 1;
    } else {
        return num * arguments.callee(num-1)
    }
}
```

在这个重写后的 `factorial()` 函数的函数体内，没有再引用函数名 `factorial`。这样，无论引用函数时使用的是什么名字，都可以保证正常完成递归调用。例如：

``` javascript
var trueFactorial = factorial;

factorial = function(){
    return 0;
};

console.log(trueFactorial(5));  // 120
console.log(factorial(5));      // 0
```

在此，变量 `trueFactorial` 获得了 `factorial` 的值，实际上是在另一个位置上保存了一个函数的指针。然后，我们又将一个简单地返回 `0` 的函数赋值给 `factorial` 变量。如果像原来的 `factorial()` 那样不使用 `arguments.callee`，调用 `trueFactorial()` 就会返回 `0`。可是，在解除了函数体内的代码与函数名的耦合状态之后，`trueFactorial()` 仍然能够正常地计算阶乘；至于 `factorial()`，它现在只是一个返回 `0` 的函数。

函数内部的另一个特殊对象是 `this`，其行为与 Java 和 C# 中的 `this` 大致类似。换句话说，`this` 引用的是函数据以执行的环境对象（当在网页的全局作用域中调用函数时，`this` 对象引用的就是 `window`）。来看下面的例子。

``` javascript
window.color = "red";
var o = { color: "blue" };

function sayColor(){
    console.log(this.color);
}
sayColor();     // "red"

o.sayColor = sayColor;
o.sayColor();   // "blue"
```

上面这个函数 `sayColor()` 是在全局作用域中定义的，它引用了 `this` 对象。由于在调用函数之前，`this` 的值并不确定，因此 `this` 可能会在代码执行过程中引用不同的对象。当在全局作用域中调用 `sayColor()` 时，`this` 引用的是全局对象 `window`；换句话说，对 `this.color` 求值会转换成对 `window.color` 求值，于是结果就返回了 `"red"`。而当把这个函数赋给对象 `o` 并调用 `o.sayColor()` 时，`this` 引用的是对象 `o`，因此对 `this.color` 求值会转换成对 `o.color` 求值，结果就返回了 `"blue"`。

请大家一定要牢记，函数的名字仅仅是一个包含指针的变量而已。因此，即使是在不同的环境中执行，全局的 `sayColor()` 函数与 `o.sayColor()` 指向的仍然是同一个函数。

ECMAScript 5也规范化了另一个函数对象的属性 `caller`。这个属性中保存着「调用当前函数的函数的引用」，如果是在全局作用域中调用当前函数，它的值为 `null`。例如：

``` javascript
function outer(){
    inner();
}

function inner(){
    console.log(arguments.callee.caller);
} 

outer();
```

以上代码会导致警告框中显示 `outer()` 函数的源代码。因为 `outer()` 调用了 `inter()`，所以 `arguments.callee.caller` 就指向 `outer()`。

在严格模式下，访问 `arguments.callee`属性，或为函数的 `caller` 属性赋值，都会导致错误。

## 函数的属性和方法

JavaScript 中的函数是对象，因此函数也有属性和方法。每个函数都包含两个属性：`length` 和 `prototype`。其中，`length` 属性表示函数希望接收的命名参数的个数，如下面的例子所示。

``` javascript
function sayName(name){
    console.log(name);
}

function sum(num1, num2){
    return num1 + num2;
}

function sayHi(){
    console.log("hi");
}

console.log(sayName.length);      // 1
console.log(sum.length);          // 2
console.log(sayHi.length);        // 0
```

对于 JavaScript 中的引用类型而言，`prototype` 是保存它们所有实例方法的真正所在。换句话说，诸如 `toString()` 和 `valueOf()` 等方法实际上都保存在 `prototype` 名下，只不过是通过各自对象的实例访问罢了。在创建自定义引用类型以及实现继承时，`prototype` 属性的作用是极为重要的。在 ECMAScript 5中，`prototype` 属性是不可枚举的，因此使用 `for-in` 无法发现。

每个函数都包含两个非继承而来的方法：`apply()` 和 `call()`。这两个方法的用途都是在特定的作用域中调用函数，实际上等于设置函数体内 `this` 对象的值。首先，`apply()` 方法接收两个参数：一个是在其中运行函数的作用域，另一个是参数数组。其中，第二个参数可以是 `Array` 的实例，也可以是 `arguments` 对象。例如：

``` javascript
function sum(num1, num2){
    return num1 + num2;
}

function callSum1(num1, num2){
    return sum.apply(this, arguments);  // 传入 arguments 对象
}

function callSum2(num1, num2){
    return sum.apply(this, [num1, num2]);  // 传入数组
}

console.log(callSum1(10,10));   // 20
console.log(callSum2(10,10));   // 20
```

在上面这个例子中，`callSum1()` 在执行 `sum()` 函数时传入了 `this`（因为是在全局作用域中调用的，所以传入的就是 `window` 对象）和 `arguments` 对象。而 `callSum2` 同样也调用了 `sum()` 函数，但它传入的则是 `this` 和一个参数数组。这两个函数都会正常执行并返回正确的结果。

`call()` 方法与 `apply()` 方法的作用相同，它们的区别仅在于接收参数的方式不同。对于 `call()` 方法而言，第一个参数是 `this` 值没有变化，变化的是其余参数都直接传递给函数。换句话说，在使用 `call()` 方法时，传递给函数的参数必须逐个列举出来，如下面的例子所示。

``` javascript
function sum(num1, num2){
    return num1 + num2;
}

function callSum(num1, num2){
    return sum.call(this, num1, num2);
}

console.log(callSum(10,10));   // 20
```

在使用 `call()` 方法的情况下，`callSum()` 必须明确地传入每一个参数。结果与使用 `apply()` 没有什么不同。至于是使用 `apply()` 还是 `call()`，完全取决于你采取哪种给函数传递参数的方式最方便。如果你打算直接传入 `arguments` 对象，或者包含函数中先接收到的也是一个数组，那么使用 `apply()` 肯定更方便；否则，选择 `call()` 可能更合适。（在不给函数传递参数的情况下，使用哪个方法都无所谓。）
事实上，传递参数并非 `apply()` 和 `call()` 真正的用武之地；它们真正强大的地方是能够扩充函数赖以运行的作用域。下面来看一个例子。

``` javascript
window.color = "red";
var o = { color: "blue" };

function sayColor(){
    console.log(this.color);
}
sayColor();                // red

sayColor.call(this);       // red
sayColor.call(window);     // red
sayColor.call(o);          // blue
```

这个例子是在前面说明 `this` 对象的示例基础上修改而成的。这一次，`sayColor()` 也是作为全局函数定义的，而且当在全局作用域中调用它时，它确实会显示 `"red"`，因为对 `this.color` 的求值会转换成对 `window.color` 的求值。而 `sayColor.call(this)` 和 `sayColor.call(window)`，则是两种显式地在全局作用域中调用函数的方式，结果当然都会显示 `"red"`。但是，当运行 `sayColor.call(o)` 时，函数的执行环境就不一样了，因为此时函数体内的 `this` 对象指向了 `o`，于是结果显示的是 `"blue"`。

使用 `call()` 或 `apply()` 来扩充作用域的最大好处，就是对象不需要与方法有任何耦合关系。在前面例子的第一个版本中，我们是先将 `sayColor()` 函数放到了对象 `o` 中，然后再通过 `o` 来调用它的；而在这里重写的例子中，就不需要先前那个多余的步骤了。

## 关卡

``` javascript
// 挑战一，合并任意个数的字符串
var concat = function(){
    // 待实现方法体
}
console.log(concat('st','on','e'));  // stone
```

``` javascript
// 挑战二，输出指定位置的斐波那契数列
var fioacciSequece = function(count){
    // 待实现方法体
}
console.log(fioacciSequece(12));  // 0、1、1、2、3、5、8、13、21、34、55、89
```

```javascript
// 挑战三，三维数组或 n 维数组去重，使用 arguments 重写
var arr = [2,3,4,[2,3,[2,3,4,2],5],3,5,[2,3,[2,3,4,2],2],4,3,6,2];
var unique = function(arr){
    // 待实现方法体
}
console.log(unique(arr)); // [2,3,4,5,6]
```

## 更多

> 关注微信公众号「劼哥舍」回复「答案」，获取关卡详解。  
> 关注 [https://github.com/stone0090/javascript-lessons](https://github.com/stone0090/javascript-lessons)，获取最新动态。

