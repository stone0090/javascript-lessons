# 关卡详解

``` javascript
// 挑战一
var k;
for(i=0, j=0; i<10, j<6; i++, j++){
    k = i + j;
}
console.log(k);  // 10
```

``` javascript
// 挑战二
var nums = [12,32,54,56,78,89];
for(var n in nums){
    console.log(n);  // 0,1,2,3,4,5
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
showCase(new String('A'));   // "Do not know!"
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
showCase(String('A'));   // "Case A","Case B"
```

``` javascript
// 挑战五
var i = 0;
for (;;) {
    if (i = 2) {
        continue;
    }
    if (i > 20) {
        break;
    }
    i++;
}
console.log(i);  // 死循环
```