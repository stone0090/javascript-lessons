# 关卡详解

请实现下面用来枚举属性的对象工具函数：

``` javascript
/*
 * 把 p 中的可枚举属性复制到 o 中，并返回 o
 * 如果 o 和 p 中含有同名属性，则覆盖 o 中的属性
 */
function extend(o, p) {
    for (prop in p) {           // 遍历 p 中的所有属性
        o[prop] = p[prop];      // 将属性添加至 o 中
    }
    return o;
}
```

``` javascript
/*
 * 将 p 中的可枚举属性复制至 o 中，并返回 o
 * 如果 o 和 p 中有同名的属性，o 中的属性将不受影响
 */
function merge(o, p) {
    for (prop in p) {                           // 遍历 p 中的所有属性
        if (o.hasOwnProperty[prop]) continue;   // 过滤掉已经在 o 中存在的属性
        o[prop] = p[prop];                      // 将属性添加至 o 中
    }
    return o;
}
```

``` javascript
/*
 * 如果 o 中的属性在 p 中没有同名属性，则从 o 中删除这个属性
 * 返回 o
 */
function restrict(o, p) {
    for (prop in o) {                      // 遍历 o 中的所有属性
        if (! (prop in p)) delete o[prop]; // 如果在 p 中不存在，则删除之
    }
    return o;
}
```

``` javascript
/*
 * 如果 o 中的属性在 p 中存在同名属性，则从 o 中删除这个属性
 * 返回 o
 */
function subtract(o, p) {
    for (prop in p) {      // 遍历 p 中的所有属性
         delete o[prop];   // 从 o 中删除（删除一个不存在的属性不会报错）
    }
    return o;
}
```

``` javascript
/*
 * 返回一个新对象，这个对象同时拥有 o 的属性和 p 的属性
 * 如果 o 和 p 中有重名属性，使用 p 中的属性值
 */
function union(o, p) { 
    return extend(extend({},o), p);
}
```

``` javascript
/*
 * 返回一个新对象，这个对象拥有同时在 o 和 p 中出现的属性
 * 很像求 o 和 p 的交集，但 p 中属性的值被忽略
 */
function intersection(o, p) { 
    return restrict(extend({},o), p);
}
```

``` javascript
/*
 * 返回一个数组，这个数组包含的是 o 中可枚举的自有属性的名字
 */
function keys(o) {
    if (typeof o !== "object") throw TypeError(); // 参数必须是对象
    var result = [];                              // 将要返回的数组
    for (var prop in o) {                         // 遍历所有可枚举的属性
         if (o.hasOwnProperty(prop))              // 判断是否是自有属性
            result.push(prop);                    // 将属性名添加至数组中
    }
    return result;                                // 返回这个数组
}
```