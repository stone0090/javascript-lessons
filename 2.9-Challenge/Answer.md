## 综合挑战详解

```javascript
// 挑战一
function Foo() {
    getName = function () { alert (1); };
    return this;
}
Foo.getName = function () { alert (2);};
Foo.prototype.getName = function () { alert (3);};
var getName = function () { alert (4);};
function getName() { alert (5);}

// 请写出以下输出结果：
Foo.getName(); // 2
getName(); // 4
Foo().getName(); // 1
getName(); // 1
new Foo.getName(); // 2
new Foo().getName(); // 3
new new Foo().getName(); // 3
```

此题是在[掘金](http://gold.xitu.io)看到的，被收藏了1000多次，考察范围广容易出错，详解请看[一道常被人轻视的前端 JS 面试题](http://gold.xitu.io/entry/580cdbeec4c9710058943151/detail)。
