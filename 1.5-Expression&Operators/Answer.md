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

``` javascript
// 挑战九
var val = 'smtg'; 
console.log('Value is ' + (val === 'smtg') ? 'Something' : 'Nothing');  // "Something"
```

``` javascript
// 挑战九
console.log(1 + - + + + - + 1);  // 2
```