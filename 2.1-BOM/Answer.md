## 关卡详解

要点：JavaScript 是一个单线程序的解释器，因此一定时间内只能执行一段代码。为了控制要执行的代码，就有一个 JavaScript 任务队列。这些任务会按照将它们添加到队列的顺序执行。`setTimeout()` 的第二个参数告诉 JavaScript 再过多长时间把当前任务添加到队列中。如果队列是空的，那么添加的代码会立即执行；如果队列不是空的，那么它就要等前面的代码执行完了以后再执行。

``` javascript
// 挑战一
setTimeout(function () {
    console.log("1");
}, 0)
console.log("2");   // "2","1"
```

``` javascript
// 挑战二
for (var i = 0;i<5;i++) {
    setTimeout(function () {
        console.log(i);     // 5,5,5,5,5
    }, 0)
};
```

``` javascript
// 挑战三
var a = 1;
var obj = {
    a : 2,
    b : function(){
        setTimeout(function () {
            console.log(this.a);
        }, 0)
    }
}
obj.b();    // 1
```

``` javascript
// 挑战四
var a = 1;
var obj = {
    a : 2,
    b : function(){
        setTimeout(function () {
            console.log(this.a);
        }.call(this), 0);
    }
}
obj.b();    // 2
```