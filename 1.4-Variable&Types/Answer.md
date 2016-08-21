# 关卡详解

``` javascript
// 挑战一
console.log(typeof "undefined");  // "string"
console.log(typeof null);         // "object"
```

``` javascript
// 挑战二
var message = "some string";
console.log(typeof massage);    // "undefined"
message = 10000;
console.log(typeof message);    // "number"
```

``` javascript
// 挑战三
var a;
var b = null;
var c = {};
if(a && b && c){
    console.log("true.");
}else{
    console.log("false.");      // false
}
```

``` javascript
// 挑战四
console.log(typeof (0 / 0));    // "number"
console.log(023 + 123);         // 142，八进制+十进制
```

``` javascript
// 挑战五
console.log(Number("1234S"));   // NaN
console.log(parseInt("1234S")); // 1234
```

``` javascript
// 挑战六
console.log(3.14E-7 === 0.000000314);   // true
console.log(0.1 + 0.6 === 0.7);         // true
console.log(0.1 + 0.7 === 0.8);         // false
console.log(NaN === NaN);               // false
```

``` javascript
// 挑战七
console.log("\right\now");          // ight
                                    // ow
console.log("\right\now".length);   // 8
console.log(010.toString(2));       // "1000"
```

``` javascript
// 挑战八
// 1、为 person、wife、child 对象新增 weight 属性，数值分别为 62、36、15。
// 2、为 person 对象新增二胎 child2 子对象，name 为 emma，其他属性自行发挥。
var person = {
    name: "stone",
    age: 30,
    weight: 62,
    wife: {
        name: "sohpie",
        age: 30,
        weight: 36
    },
    child:{
        name: "tommy",
        age: 3,
        weight: 15
    },
    child2:{
        name: "emma",
        age: 1,
        weight: 6
    }
}
```

