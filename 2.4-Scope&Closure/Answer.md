# 关卡详解

只要你理解了该章节的3个要点，就能轻松解答以下习题了。

- 局部变量的优先级高于同名的全局变量。
- 函数可以记忆它被创建时候的环境，可以读取上层函数内部的变量。
- `this` 指代的是调用函数的那个对象，如无指代的对象，则为全局对象。

```javascript
// 挑战一
function func1() {
    function func2() {
        console.log(this)
    }
    return func2;
}
func1()();  // ???
```

挑战一解析见[闭包那一章有疑惑，望解答 #14](https://github.com/stone0090/javascript-lessons/issues/14)。


```javascript
// 挑战二
scope = "stone";

function Func() {
    var scope = "sophie";

    function inner() {
        console.log(scope);
    }
    return inner;
}

var ret = Func();
ret();    // "sophie"
```

```javascript
// 挑战三
scope = "stone";

function Func() {
    var scope = "sophie";

    function inner() {
        console.log(scope);
    }
    scope = "tommy";
    return inner;
}

var ret = Func();
ret();    // "tommy"
```

- 挑战三解析，虽然在内部函数 `inner()` 被创建时，局部变量 `scope` 的值为 `"sophie"`，但当函数执行到 `return inner` 时，局部变量 `scope` 的值已被更新为 `"tommy"`，所以最终输出的结果为 `"tommy"`。
- 注意：`inner()` 函数的局部变量 `scope` 记录的并不是创建瞬间的值，而是指向 `Func()` 函数的局部变量的 `scope` 的指针，「内部函数的变量」会随着「外层函数的变量」的值一起发生改变。

```javascript
// 挑战四
scope = "stone";

function Bar() {
    console.log(scope);
}

function Func() {
    var scope = "sophie";
    return Bar;
}

var ret = Func();
ret();    // "stone"
```

```javascript
// 挑战五
var name = "The Window";　　
var object = {　　　　
    name: "My Object",
    getNameFunc: function() {　　　　　　
        return function() {　　　　　　　　
            return this.name;　　　　　　
        };　　　　
    }　　
};　　
console.log(object.getNameFunc()());    // "The Window"
```

```javascript
// 挑战六
var name = "The Window";　　
var object = {　　　　
    name: "My Object",
    getNameFunc: function() {　　　　　　
        var that = this;　　　　　　
        return function() {　　　　　　　　
            return that.name;　　　　　　
        };　　　　
    }　　
};　　
console.log(object.getNameFunc()());    // "My Object"
```