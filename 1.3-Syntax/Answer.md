# 关卡详解

请判断以下代码是否有效？如果有效请给出结果，如果无效请说明原因。

``` javascript
// 挑战一
var class = 'person';
console.log(class);     // 无效，因为 class 是关键字。
```

``` javascript
// 挑战二
var Class = 'person';
console.log(class);     // 无效，虽然 Class 是合法的标识符，但是输出的是 class。
```

``` javascript
// 挑战三
var True = false;
console.log(True);      // false，有效，True 是合法的标识符。
```

``` javascript
// 挑战四
var true = false;
console.log(True);      // 无效，因为 true 是关键字。
```

``` javascript
// 挑战五
var $_$ = 'stone';
console.log($_$);       // 'stone'，有效，$_$ 是合法的标识符。
```

``` javascript
// 挑战六
var 00_name = 'stone';
console.log(00_name);   // 无效，标识符不能以数字开头。
```

``` javascript
// 挑战七
var Array = 'null';
console.log(Array);     // 'null'，有效，虽然 Array 是全局变量，但可以被重写，Array 被重写之后就不能再用当做构造函数使用了。
```

``` javascript
// 挑战八
var undefined = 'null';
console.log(undefined); // undefined，有效，虽然没报错，但是没有赋值成功。
```

``` javascript
// 挑战九
var result = 1 + 2
+ 3 + 4
console.log(result);    // 10，有效。
```