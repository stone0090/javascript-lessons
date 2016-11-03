## 综合挑战

本章节收集各种各样坑人无数的 JavaScript 题目，保证虐得大家不要不要的，祝大家好运。

> 如果你还遇到过其他特别坑的 JavaScript 题目欢迎 PR 给我，让我们一起虐爆其他人。

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
Foo.getName();
getName();
Foo().getName();
getName();
new Foo.getName();
new Foo().getName();
new new Foo().getName();
```


## 更多

> 关注微信公众号「劼哥舍」回复「答案」，获取关卡详解。  
> 关注 [https://github.com/stone0090/javascript-lessons](https://github.com/stone0090/javascript-lessons)，获取最新动态。