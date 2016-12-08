# 原型及原型链

原型链是一种机制，指的是 JavaScript 每个对象都有一个内置的 `__proto__` 属性指向创建它的构造函数的 `prototype`（原型）属性。原型链的作用是为了实现对象的继承，要理解原型链，需要先从**函数对象**、`constructor`、`new`、`prototype`、`__proto__` 这五个概念入手。

## 函数对象

前面讲过，在 JavaScript 里，函数即对象，程序可以随意操控它们。比如，可以把函数赋值给变量，或者作为参数传递给其他函数，也可以给它们设置属性，甚至调用它们的方法。下面示例代码对「普通对象」和「函数对象」进行了区分。

普通对象：

```javascript
var o1 = {};
var o2 = new Object();
```

函数对象：

```javascript
function f1(){};
var f2 = function(){};
var f3 = new Function('str','console.log(str)');
```

简单的说，凡是使用 `function` 关键字或 `Function` 构造函数创建的对象都是函数对象。而且，只有函数对象才拥有  `prototype` （原型）属性。

## `constructor` 构造函数

函数还有一种用法，就是把它作为构造函数使用。像 `Object` 和 `Array` 这样的原生构造函数，在运行时会自动出现在执行环境中。此外，也可以创建自定义的构造函数，从而自定义对象类型的属性和方法。如下代码所示：

```javascript
function Person(name, age, job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = function(){
        console.log(this.name);
    };
}

var person1 = new Person("Stone", 28, "Software Engineer");
var person2 = new Person("Sophie", 29, "English Teacher");
```

在这个例子中，我们创建了一个自定义构造函数 `Person()`，并通过该构造函数创建了两个普通对象 `person1` 和 `person2`，这两个普通对象均包含3个属性和1个方法。

你应该注意到函数名 `Person` 使用的是大写字母 `P`。按照惯例，构造函数始终都应该以一个大写字母开头，而非构造函数则应该以一个小写字母开头。这个做法借鉴自其他面向对象语言，主要是为了区别于 JavaScript 中的其他函数；因为构造函数本身也是函数，只不过可以用来创建对象而已。

## `new` 操作符

要创建 `Person` 的新实例，必须使用 `new` 操作符。以这种方式调用构造函数实际上会经历以下4个步骤：

1. 创建一个新对象；
2. 将构造函数的作用域赋给新对象（因此 `this` 就指向了这个新对象）；
3. 执行构造函数中的代码（为这个新对象添加属性）；
4. 返回新对象。

### 将构造函数当作函数

构造函数与其他函数的唯一区别，就在于调用它们的方式不同。不过，构造函数毕竟也是函数，不存在定义构造函数的特殊语法。任何函数，只要通过 `new` 操作符来调用，那它就可以作为构造函数；而任何函数，如果不通过 `new` 操作符来调用，那它跟普通函数也不会有什么两样。例如，前面例子中定义的 `Person()` 函数可以通过下列任何一种方式来调用。

```javascript
// 当作构造函数使用
var person = new Person("Stone", 28, "Software Engineer");
person.sayName(); // "Stone"

// 作为普通函数调用
Person("Sophie", 29, "English Teacher"); // 添加到 window
window.sayName(); // "Sophie"

// 在另一个对象的作用域中调用
var o = new Object();
Person.call(o, "Tommy", 3, "Baby");
o.sayName(); // "Tommy"
```

这个例子中的前两行代码展示了构造函数的典型用法，即使用 `new` 操作符来创建一个新对象。接下来的两行代码展示了不使用 `new` 操作符调用 `Person()` 会出现什么结果，属性和方法都被添加给 `window` 对象了。当在全局作用域中调用一个函数时，`this` 对象总是指向 `Global` 对象（在浏览器中就是 `window` 对象）。因此，在调用完函数之后，可以通过 `window` 对象来调用 `sayName()` 方法，并且还返回了 `"Sophie"` 。最后，也可以使用 `call()`（或者 `apply()`）在某个特殊对象的作用域中调用 `Person()` 函数。这里是在对象 `o` 的作用域中调用的，因此调用后 `o` 就拥有了所有属性和 `sayName()` 方法。

### 构造函数的问题

构造函数模式虽然好用，但也并非没有缺点。使用构造函数的主要问题，就是每个方法都要在每个实例上重新创建一遍。在前面的例子中，`person1` 和 `person2` 都有一个名为 `sayName()` 的方法，但那两个方法不是同一个 `Function` 的实例。因为 JavaScript 中的函数是对象，因此每定义一个函数，也就是实例化了一个对象。从逻辑角度讲，此时的构造函数也可以这样定义。

```javascript
function Person(name, age, job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = new Function("console.log(this.name)"); // 与声明函数在逻辑上是等价的
}
```

从这个角度上来看构造函数，更容易明白每个 `Person` 实例都包含一个不同的 `Function` 实例（`sayName()` 方法）。说得明白些，以这种方式创建函数，虽然创建 `Function` 新实例的机制仍然是相同的，但是不同实例上的同名函数是不相等的，以下代码可以证明这一点。

```javascript
console.log(person1.sayName == person2.sayName);  // false
```

然而，创建两个完成同样任务的 `Function` 实例的确没有必要；况且有 `this` 对象在，根本不用在执行代码前就把函数绑定到特定对象上面。因此，大可像下面这样，通过把函数定义转移到构造函数外部来解决这个问题。

```javascript
function Person(name, age, job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = sayName;
}

function sayName(){
    console.log(this.name);
}

var person1 = new Person("Stone", 28, "Software Engineer");
var person2 = new Person("Sophie", 29, "English Teacher");
```

在这个例子中，我们把 `sayName()` 函数的定义转移到了构造函数外部。而在构造函数内部，我们将 `sayName` 属性设置成等于全局的 `sayName` 函数。这样一来，由于 `sayName` 包含的是一个指向函数的指针，因此 `person1` 和 `person2` 对象就共享了在全局作用域中定义的同一个 `sayName()` 函数。这样做确实解决了两个函数做同一件事的问题，可是新问题又来了，在全局作用域中定义的函数实际上只能被某个对象调用，这让全局作用域有点名不副实。而更让人无法接受的是，如果对象需要定义很多方法，那么就要定义很多个全局函数，于是我们这个自定义的引用类型就丝毫没有封装性可言了。好在，这些问题可以通过使用原型来解决。

## `prototype` 原型

我们创建的每个函数都有一个 `prototype`（原型）属性。使用原型的好处是可以让所有对象实例共享它所包含的属性和方法。换句话说，不必在构造函数中定义对象实例的信息，而是可以将这些信息直接添加到原型中，如下面的例子所示。

```javascript
function Person(){}

Person.prototype.name = "Stone";
Person.prototype.age = 28;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function(){
    console.log(this.name);
};

var person1 = new Person();
person1.sayName();   // "Stone"

var person2 = new Person();
person2.sayName();   // "Stone"

console.log(person1.sayName == person2.sayName);  // true
```

在此，我们将 `sayName()` 方法和所有属性直接添加到了 `Person` 的 `prototype` 属性中，构造函数变成了空函数。即使如此，也仍然可以通过调用构造函数来创建新对象，而且新对象还会具有相同的属性和方法。但与前面的例子不同的是，新对象的这些属性和方法是由所有实例共享的。换句话说，`person1` 和 `person2` 访问的都是同一组属性和同一个 `sayName()` 函数。

### 理解原型对象

在默认情况下，所有原型对象都会自动获得一个 `constructor`（构造函数）属性，这个属性包含一个指向 `prototype` 属性所在函数的指针。就拿前面的例子来说，`Person.prototype.constructor` 指向 `Person`。而通过这个构造函数，我们还可继续为原型对象添加其他属性和方法。

虽然可以通过对象实例访问保存在原型中的值，但却不能通过对象实例重写原型中的值。如果我们在实例中添加了一个属性，而该属性与实例原型中的一个属性同名，那我们就在实例中创建该属性，该属性将会屏蔽原型中的那个属性。来看下面的例子。

```javascript
function Person(){}

Person.prototype.name = "Stone";
Person.prototype.age = 28;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function(){
    console.log(this.name);
};

var person1 = new Person();
var person2 = new Person();

person1.name = "Sophie";
console.log(person1.name);     // "Sophie"，来自实例
console.log(person2.name);     // "Stone"，来自原型

```

在这个例子中，`person1` 的 `name` 被一个新值给屏蔽了。但无论访问 `person1.name` 还是访问 `person2.name` 都能够正常地返回值，即分别是 `"Sophie"`（来自对象实例）和 `"Stone"`（来自原型）。当访问 `person1.name` 时，需要读取它的值，因此就会在这个实例上搜索一个名为 `name` 的属性。这个属性确实存在，于是就返回它的值而不必再搜索原型了。当访问 `person2. name` 时，并没有在实例上发现该属性，因此就会继续搜索原型，结果在那里找到了 `name` 属性。

当为对象实例添加一个属性时，这个属性就会**屏蔽**原型中保存的同名属性；换句话说，添加这个属性只会阻止我们访问原型中的那个属性，但不会修改那个属性。即使将这个属性设置为 `null` ，也只会在实例中设置这个属性，而不会恢复其指向原型的连接。不过，使用 `delete` 操作符则可以完全删除实例属性，从而让我们能够重新访问原型中的属性，如下所示。

```javascript
function Person(){}

Person.prototype.name = "Stone";
Person.prototype.age = 28;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function(){
    console.log(this.name);
};

var person1 = new Person();
var person2 = new Person();

person1.name = "Sophie";
console.log(person1.name);     // "Sophie"，来自实例
console.log(person2.name);     // "Stone"，来自原型

delete person1.name;
console.log(person1.name);     // "Stone"，来自原型
```

在这个修改后的例子中，我们使用 `delete` 操作符删除了 `person1.name`，之前它保存的 `"Sophie"` 值屏蔽了同名的原型属性。把它删除以后，就恢复了对原型中 `name` 属性的连接。因此，接下来再调用 `person1.name` 时，返回的就是原型中 `name` 属性的值了。

### 更简单的原型语法

前面例子中每添加一个属性和方法就要敲一遍 `Person.prototype`。为减少不必要的输入，也为了从视觉上更好地封装原型的功能，更常见的做法是用一个包含所有属性和方法的对象字面量来重写整个原型对象，如下面的例子所示。

```javascript
function Person(){}

Person.prototype = {
    name : "Stone",
    age : 28,
    job: "Software Engineer",
    sayName : function () {
        console.log(this.name);
    }
};
```

在上面的代码中，我们将 `Person.prototype` 设置为等于一个以对象字面量形式创建的新对象。最终结果相同，但有一个例外：`constructor` 属性不再指向 `Person` 了。前面曾经介绍过，每创建一个函数，就会同时创建它的 `prototype` 对象，这个对象也会自动获得 `constructor` 属性。而我们在这里使用的语法，本质上完全重写了默认的 `prototype` 对象，因此 `constructor` 属性也就变成了新对象的 `constructor` 属性（指向 `Object` 构造函数），不再指向 `Person` 函数。此时，尽管 `instanceof` 操作符还能返回正确的结果，但通过 `constructor` 已经无法确定对象的类型了，如下所示。

```javascript
var friend = new Person();

console.log(friend instanceof Object);        // true
console.log(friend instanceof Person);        // true
console.log(friend.constructor === Person);    // false
console.log(friend.constructor === Object);    // true
```

在此，用 `instanceof` 操作符测试 `Object` 和 `Person` 仍然返回 `true`，但 `constructor` 属性则等于 `Object` 而不等于 `Person` 了。如果 `constructor` 的值真的很重要，可以像下面这样特意将它设置回适当的值。

```javascript
function Person(){}

Person.prototype = {
    constructor : Person,
    name : "Stone",
    age : 28,
    job: "Software Engineer",
    sayName : function () {
        console.log(this.name);
    }
};
```

以上代码特意包含了一个 `constructor` 属性，并将它的值设置为 `Person` ，从而确保了通过该属性能够访问到适当的值。

注意，以这种方式重设 `constructor` 属性会导致它的 `[[Enumerable]]` 特性被设置为 `true`。默认情况下，原生的 `constructor` 属性是不可枚举的，因此如果你使用兼容 ECMAScript 5 的 JavaScript 引擎，可以试一试 `Object.defineProperty()`。

```javascript
function Person(){}

Person.prototype = {
    name : "Stone",
    age : 28,
    job : "Software Engineer",
    sayName : function () {
        console.log(this.name);
    }
}; 

// 重设构造函数，只适用于 ECMAScript 5 兼容的浏览器
Object.defineProperty(Person.prototype, "constructor", {
    enumerable: false,
    value: Person
});
```

### 原型的动态性

由于在原型中查找值的过程是一次搜索，因此我们对原型对象所做的任何修改都能够立即从实例上反映出来，即使是先创建了实例后修改原型也照样如此。请看下面的例子。

```javascript
var friend = new Person();

Person.prototype.sayHi = function(){
    console.log("hi");
};

friend.sayHi();   // "hi"（没有问题！）
```

以上代码先创建了 `Person` 的一个实例，并将其保存在 `friend` 中。然后，下一条语句在 `Person.prototype` 中添加了一个方法 `sayHi()`。即使 `person` 实例是在添加新方法之前创建的，但它仍然可以访问这个新方法。其原因可以归结为实例与原型之间的松散连接关系。当我们调用 `friend.sayHi()` 时，首先会在实例中搜索名为 `sayHi` 的属性，在没找到的情况下，会继续搜索原型。因为实例与原型之间的连接只不过是一个指针，而非一个副本，因此就可以在原型中找到新的 `sayHi` 属性并返回保存在那里的函数。

尽管可以随时为原型添加属性和方法，并且修改能够立即在所有对象实例中反映出来，但如果是重写整个原型对象，那么情况就不一样了。我们知道，调用构造函数时会为实例添加一个指向最初原型的 `[[Prototype]]` 指针，而把原型修改为另外一个对象就等于切断了构造函数与最初原型之间的联系。请记住：实例中的指针仅指向原型，而不指向构造函数。看下面的例子。

```javascript
function Person(){}

var friend = new Person();

Person.prototype = {
    constructor: Person,
    name : "Stone",
    age : 28,
    job : "Software Engineer",
    sayName : function () {
        console.log(this.name);
    }
};

friend.sayName();   // Uncaught TypeError: friend.sayName is not a function
```

在这个例子中，我们先创建了 `Person` 的一个实例，然后又重写了其原型对象。然后在调用 `friend.sayName()` 时发生了错误，因为 `friend` 指向的是重写前的原型对象，其中并不包含以该名字命名的属性。

### 原生对象的原型

原型的重要性不仅体现在创建自定义类型方面，就连所有原生的引用类型，都是采用这种模式创建的。所有原生引用类型（`Object`、`Array`、`String`，等等）都在其构造函数的原型上定义了方法。例如，在 `Array.prototype` 中可以找到 `sort()` 方法，而在 `String.prototype` 中可以找到 `substring()` 方法，如下所示。

```javascript
console.log(typeof Array.prototype.sort);       // "function"
console.log(typeof String.prototype.substring); // "function"
```

通过原生对象的原型，不仅可以取得所有默认方法的引用，而且也可以定义新方法。可以像修改自定义对象的原型一样修改原生对象的原型，因此可以随时添加方法。下面的代码就给基本包装类型 `String` 添加了一个名为 `startsWith()` 的方法。

```javascript
String.prototype.startsWith = function (text) {
    return this.indexOf(text) === 0;
};

var msg = "Hello world!";
console.log(msg.startsWith("Hello"));   // true
```

这里新定义的 `startsWith()` 方法会在传入的文本位于一个字符串开始时返回 `true`。既然方法被添加给了 `String.prototype` ，那么当前环境中的所有字符串就都可以调用它。由于 `msg` 是字符串，而且后台会调用 `String` 基本包装函数创建这个字符串，因此通过 `msg` 就可以调用 `startsWith()` 方法。

尽管可以这样做，但我们不推荐在产品化的程序中修改原生对象的原型。如果因某个实现中缺少某个方法，就在原生对象的原型中添加这个方法，那么当在另一个支持该方法的实现中运行代码时，就可能会导致命名冲突。而且，这样做也可能会意外地重写原生方法。

### 原型对象的问题

原型模式也不是没有缺点。首先，它省略了为构造函数传递初始化参数这一环节，结果所有实例在默认情况下都将取得相同的属性值。虽然这会在某种程度上带来一些不方便，但还不是原型的最大问题。原型模式的最大问题是由其共享的本性所导致的。

原型中所有属性是被很多实例共享的，这种共享对于函数非常合适。对于那些包含基本值的属性倒也说得过去，毕竟（如前面的例子所示），通过在实例上添加一个同名属性，可以隐藏原型中的对应属性。然而，对于包含引用类型值的属性来说，问题就比较突出了。来看下面的例子。

```javascript
function Person(){}

Person.prototype = {
    constructor: Person,
    name : "Stone",
    age : 28,
    job : "Software Engineer",
    friends : ["ZhangSan", "LiSi"],
    sayName : function () {
        console.log(this.name);
    }
};

var person1 = new Person();
var person2 = new Person();

person1.friends.push("WangWu");

console.log(person1.friends);    // "ZhangSan,LiSi,WangWu"
console.log(person2.friends);    // "ZhangSan,LiSi,WangWu"
console.log(person1.friends === person2.friends);  // true
```

在此，`Person.prototype` 对象有一个名为 `friends` 的属性，该属性包含一个字符串数组。然后，创建了 `Person` 的两个实例。接着，修改了 `person1.friends` 引用的数组，向数组中添加了一个字符串。由于 `friends` 数组存在于 `Person.prototype` 而非 `person1` 中，所以刚刚提到的修改也会通过 `person2.friends`（与 `person1.friends` 指向同一个数组）反映出来。假如我们的初衷就是像这样在所有实例中共享一个数组，那么对这个结果我没有话可说。可是，实例一般都是要有属于自己的全部属性的。

### 构造函数和原型结合

所以，构造函数用于定义实例属性，而原型用于定义方法和共享的属性。结果，每个实例都会有自己的一份实例属性的副本，但同时又共享着对方法的引用，最大限度地节省了内存。下面的代码重写了前面的例子。

```javascript
function Person(name, age, job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.friends = ["ZhangSan", "LiSi"];
}

Person.prototype = {
    constructor : Person,
    sayName : function(){
        console.log(this.name);
    }
}

var person1 = new Person("Stone", 28, "Software Engineer");
var person2 = new Person("Sophie", 29, "English Teacher");

person1.friends.push("WangWu");
console.log(person1.friends);    // "ZhangSan,LiSi,WangWu"
console.log(person2.friends);    // "ZhangSan,LiSi"
console.log(person1.friends === person2.friends);    // false
console.log(person1.sayName === person2.sayName);    // true
```

在这个例子中，实例属性都是在构造函数中定义的，而由所有实例共享的属性 `constructor` 和方法 `sayName()` 则是在原型中定义的。而修改了 `person1.friends`（向其中添加一个新字符串），并不会影响到 `person2.friends`，因为它们分别引用了不同的数组。

这种构造函数与原型混成的模式，是目前在 JavaScript 中使用最广泛、认同度最高的一种创建自定义类型的方法。可以说，这是用来定义引用类型的一种默认模式。

## `__proto__`

为什么在构造函数的 `prototype` 中定义了属性和方法，它的实例中就能访问呢？

那是因为当调用构造函数创建一个新实例后，该实例的内部将包含一个指针 `__proto__`，指向构造函数的原型。Firefox、Safari 和 Chrome 的每个对象上都有这个属性 ，而在其他浏览器中是完全不可见的（为了确保浏览器兼容性问题，不要直接使用 `__proto__` 属性，此处只为解释原型链而演示）。让我们来看下面代码和图片：

![](http://qiniu.shijiajie.com/blog/javascript-lesson/2.5/2.jpg?1)

图中展示了 `Person` 构造函数、`Person` 的原型属性以及 `Person` 现有的两个实例之间的关系。在此，`Person.prototype.constructor` 指回了 `Person`。`Person.prototype` 中除了包含 `constructor` 属性之外，还包括后来添加的其他属性。此外，要格外注意的是，虽然这两个实例都不包含属性和方法，但我们却可以调用 `person1.sayName()`。这是因为内部指针 `__proto__` 指向 `Person.prototype`，而在 `Person.prototype` 中能找到 `sayName()` 方法。

我们来证实一下，`__proto__` 是不是真的指向 `Person.prototype` 的？如下代码所示：

```javascript
function Person(){}
var person = new Person();
console.log(person.__proto__ === Person.prototype); // true
```

既然，`__proto__` 确实是指向 `Person.prototype`，那么使用 `new` 操作符创建对象的过程可以演变为，为实例对象的 `__proto__` 赋值的过程。如下代码所示：

```javascript
function Person(){}

// var person = new Person(); 
// 上一行代码等同于以下过程 ==> 
var person = {};
person.__proto__ = Person.prototype;
Person.call(person);
```

这个例子中，我先创建了一个空对象 `person`，然后把 `person.__proto__` 指向了 `Person` 的原型对象，便继承了 `Person` 原型对象中的所有属性和方法，最后又以 `person` 为作用域执行了 `Person` 函数，`person` 便就拥有了 `Person` 的所有属性和方法。这个过程和 `var person = new Person();` 完全一样。

简单来说，当我们访问一个对象的属性时，如果这个属性不存在，那么就会去 `__proto__` 里找，这个 `__proto__` 又会有自己的 `__proto__`，于是就这样一直找下去，直到找到为止。在找不到的情况下，搜索过程总是要一环一环地前行到原型链末端才会停下来。

## 原型链

JavaScript 中描述了**原型链**的概念，并将原型链作为实现继承的主要方法。其基本思想是利用原型让一个引用类型继承另一个引用类型的属性和方法。简单回顾一下构造函数、原型和实例的关系：每个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针，而实例都包含一个指向原型对象的内部指针。如下图所示：（图源：[segmentfault.com](https://segmentfault.com/q/1010000005182807?_ea=1284630)，作者：[manxisuo](https://segmentfault.com/u/manxisuo)）

![](http://qiniu.shijiajie.com/blog/javascript-lesson/2.5/3.png)

那么，假如我们让原型对象等于另一个类型的实例，结果会怎么样呢？显然，此时的原型对象将包含一个指向另一个原型的指针，相应地，另一个原型中也包含着一个指向另一个构造函数的指针。假如另一个原型又是另一个类型的实例，那么上述关系依然成立，如此层层递进，就构成了实例与原型的链条。这就是所谓原型链的基本概念。

上面这段话比较绕口，代码更容易理解，让我们来看看实现原型链的基本模式。如下代码所示：

```javascript
function Father(){
    this.value = true;
}
Father.prototype.getValue = function(){
    return this.value;
};

function Son(){
    this.value2 = false;
}

// 继承了 Father
Son.prototype = new Father();

Son.prototype.getValue2 = function (){
    return this.value2;
};

var son = new Son();
console.log(son.getValue());  // true
```

以上代码定义了两个类型：`Father` 和 `Son`。每个类型分别有一个属性和一个方法。它们的主要区别是 `Son` 继承了 `Father`，而继承是通过创建 `Father` 的实例，并将该实例赋给 `Son.prototype` 实现的。实现的本质是重写原型对象，代之以一个新类型的实例。换句话说，原来存在于 `Father` 的实例中的所有属性和方法，现在也存在于 `Son.prototype` 中了。在确立了继承关系之后，我们给 `Son.prototype` 添加了一个方法，这样就在继承了 `Father` 的属性和方法的基础上又添加了一个新方法。

我们再用 `__proto__` 重写上面代码，更便于大家的理解：

```javascript
function Father(){
    this.value = true;
}
Father.prototype.getValue = function(){
    return this.value;
};

function Son(){
    this.value2 = false;
}

// 继承了 Father
// Son.prototype = new Father(); ==>
Son.prototype = {};
Son.prototype.__proto__ = Father.prototype;
Father.call(Son.prototype);

Son.prototype.getValue2 = function (){
    return this.value2;
};

// var son = new Son(); ==>
var son = {};
son.__proto__ = Son.prototype;
Son.call(son);

console.log(son.getValue()); // true
console.log(son.getValue === son.__proto__.__proto__.getValue); // true 
```

从以上代码可以看出，实例 `son` 调用 `getValue()` 方法，实际是经过了 `son.__proto__.__proto__.getValue` 的过程的，其中 `son.__proto__` 等于 `Son.prototype`，而 `Son.prototype.__proto__` 又等于 `Father.prototype`，所以 `son.__proto__.__proto__.getValue` 其实就是 `Father.prototype.getValue`。

事实上，前面例子中展示的原型链还少一环。我们知道，所有引用类型默然都继承了 `Obeject`，而这个继承也是通过原型链实现的。大家要记住，所有函数的默认原型都是 `Object` 的实例，因此默认原型都会包含一个内部指针 `__proto__`，指向 `Object.prototype`。这也正是所有自定义类型都会继承 `toString()`、`valueOf()` 等默认方法的根本原因。

下图展示了原型链实现继承的全部过程。（图源：[segmentfault.com](https://segmentfault.com/q/1010000005182807?_ea=1284630)，作者：[manxisuo](https://segmentfault.com/u/manxisuo)）

![](http://qiniu.shijiajie.com/blog/javascript-lesson/2.5/4.png)

上图中，`p` 指 `prototype` 属性，`[p]` 即 `__proto__` 指对象的原型，`[p]` 形成的链（虚线部分）就是**原型链**。从图中可以得出以下信息：

- `Object.prototype` 是顶级对象，所有对象都继承自它。
- `Object.prototype.__proto__ === null` ，说明原型链到 `Object.prototype` 终止。
- `Function.__proto__` 指向 `Function.prototype`。

## 关卡

根据描述写出对应的代码。

```javascript
// 挑战一
// 1.定义一个构造函数 Animal，它有一个 name 属性，以及一个 eat() 原型方法。
// 2.eat() 的方法体为：console.log(this.name + " is eating something.")。
// 3.new 一个 Animal 的实例 tiger，然后调用 eat() 方法。
// 4.用 __proto__ 模拟 new Animal() 的过程，然后调用 eat() 方法。

var Animal = function(name){
	// 待补充的代码
};

var tiger = new Animal("tiger");
// 待补充的代码

var tiger2 = {};
// 待补充的代码
```
```javascript
// 挑战二
// 1.定义一个构造函数 Bird，它继承自 Animal，它有一个 name 属性，以及一个 fly() 原型方法。
// 2.fly() 的方法体为：console.log(this.name + " want to fly higher.");。
// 3.new 一个 Bird 的实例 pigeon，然后调用 eat() 和 fly() 方法。
// 4.用 __proto__ 模拟 new Bird() 的过程，然后用代码解释 pigeon2 为何能调用 eat() 方法。

var Bird = function(name){
  	// 待补充的代码
}

var pigeon = new Bird("pigeon");
// 待补充的代码

var pigeon2 = {};
// 待补充的代码
```

```javascript
// 挑战三
// 1.定义一个构造函数 Swallow，它继承自 Bird，它有一个 name 属性，以及一个 nesting() 原型方法。
// 2.nesting() 的方法体为：console.log(this.name + " is nesting now.");。
// 3.new 一个 Swallow 的实例 yanzi，然后调用 eat()、fly() 和 nesting() 方法。
// 4.用 __proto__ 模拟 new Swallow() 的过程，然后用代码解释 yanzi2 为何能调用 eat() 方法。

var Swallow = function(name){
  	// 待补充的代码
}

var yanzi = new Swallow("yanzi");
// 待补充的代码

var yanzi2 = {};
// 待补充的代码
```

## 更多

> 关注微信公众号「劼哥舍」回复「答案」，获取关卡详解。  
> 关注 [https://github.com/stone0090/javascript-lessons](https://github.com/stone0090/javascript-lessons)，获取最新动态。