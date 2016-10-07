# 关卡详解

```javascript
// 挑战一（一维数组）
var arr = [2,3,4,2,3,5,6,4,3,2];
var unique = function(arr) {
    var result = [];
    arr.forEach(function(item){
        if(result.indexOf(item) < 0){
            result.push(item);
        }
    });
    return result;
}
console.log(unique(arr)); // [2,3,4,5,6]
```

```javascript
// 挑战二（二维数组）
var arr = [2,3,4,[2,3,4,5],3,5,[2,3,4,2],4,3,6,2];
var unique = function(arr){
    var result = [];
    arr.forEach(function(item) {
        if (Array.isArray(item)) {
            item.forEach(function(i) {
                if (result.indexOf(i) < 0) {
                    result.push(i);
                }
            });
        } else {
            if (result.indexOf(item) < 0) {
                result.push(item);
            }
        }
    });
    return result;
}
console.log(unique(arr)); // [2,3,4,5,6]
```

```javascript
// 挑战三（三维数组或 n 维数组）
var arr = [2,3,4,[2,3,[2,3,4,2],5],3,5,[2,3,[2,3,4,2],2],4,3,6,2];
var unique = function(arr) {
    var result = [];
    var f = function(i) {
        i.forEach(function(item) {
            if (Array.isArray(item)) {
                f(item);
            } else {
                if (result.indexOf(item) < 0) {
                    result.push(item);
                }
            }
        });
    };
    f(arr);
    return result;
}
console.log(unique(arr)); // [2,3,4,5,6]
```
