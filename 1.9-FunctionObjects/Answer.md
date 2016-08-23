# 关卡详解

``` javascript
// 挑战一，合并任意个数的字符串
var concat = function(){
    var result = '';
    for(var i = 0; i < arguments.length; i ++){
        result += arguments[i];
    }
    return result;
}
console.log(concat('st','on','e'));  // stone
```

``` javascript
// 挑战二，输出指定位置的斐波那契数列
var fioacciSequece = function(count){
    return (function(n) {
        if (n <= 1) {
            return n;
        } else {
            return arguments.callee(n - 1) + arguments.callee(n - 2);
        }
    }(--count));
}
console.log(fioacciSequece(12));  // 0、1、1、2、3、5、8、13、21、34、55、[89]
```

```
// 挑战三，三维数组或 n 维数组去重，使用 arguments 重写
var arr = [2,3,4,[2,3,[2,3,4,2],5],3,5,[2,3,[2,3,4,2],2],4,3,6,2];
var unique = function(arr) {
    var result = [];
    (function(arr) {
        var f = arguments.callee;
        arr.forEach(function() {
            if (Array.isArray(arguments[0])) {
                f(arguments[0]);
            } else {
                if (result.indexOf(arguments[0]) < 0) {
                    result.push(arguments[0]);
                }
            }
        });
    }(arr));
    return result;
}
console.log(unique(arr)); // [2,3,4,5,6]
```