# 关卡详解

``` javascript
// 挑战一
var x=1;
if(!!function f(){}){
    x+=typeof f;
}
console.log(x);     // "1undefined"
```

``` javascript
// 挑战二
(function f(f){
    console.log(typeof f());    // "number"
})(function(){return 1;});
```

``` javascript
// 挑战三
console.log(typeof 2*3);    // NaN
console.log(typeof 2+3);    // "number3"
```

``` javascript
// 挑战四
var a=0,b=0;
console.log(a+++b);     // 0
console.log(a);         // 1
console.log(b);         // 0
```

``` javascript
// 挑战五
var a,b,c;
a=b==c;
console.log(a);     // true
```

``` javascript
// 挑战六
console.log(1 && 3);            // 3
console.log(1 && "foo" || 0);   // "foo"
console.log(1 || "foo" && 0);   // 1
```

``` javascript
// 挑战七
var a=1;
var b=(a=(2,4,6))+a++
console.log(b);     // 12
```

``` javascript
// 挑战八
if (!("a" in window)) {
    var a = 1;
}
console.log(a);     // undefined
```

> 挑战八解析：由于受 JavaScript 引擎的变量提升机制所影响，变量 `a` 在进入 `if` 语句之前就已经被定义。我把上述代码进行简单改造，大家就能很容易理解，变量 `a` 为什么输出 `undefined`。如下所示：
``` javascript
var a;
if (!("a" in window)) {
    a = 1;
}
console.log(a);     // undefined
```

``` javascript
// 挑战九
var val = 'smtg'; 
console.log('Value is ' + (val === 'smtg') ? 'Something' : 'Nothing');  // "Something"
```

> 挑战九解析：由于条件运算符 `?:` 的优先级小于 `+` 和 `===`，所以在上述代码中，会先执行 `'Value is ' + (val === 'smtg')`，再执行条件运算符判断语句，所以最后会输出 `"Something"`。


``` javascript
// 挑战十
console.log(1 + - + + + - + 1);  // 2
```