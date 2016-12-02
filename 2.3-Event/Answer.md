## 关卡解答

```javascript
var EventUtil = {
    /**
     * 添加事件处理程序
     * @param {Element} element - 要操作的元素
     * @param {String} type - 事件名称
     * @param {Function} handler - 事件处理程序函数
     */
    addHandler: function (element, type, handler) {
        if (element.addEventListener) {//DOM2级事件处理程序
            element.addEventListener(type, handler, false);
        } else if (element.attachEvent) {//IE事件处理程序
            element.attachEvent('on' + type, handler);
        } else {//DOM0级事件处理程序
            element['on' + type] = handler;
        }
    },
    /**
     * 移除事件处理程序
     * @param {Element} element - 要操作的元素
     * @param {String} type - 事件名称
     * @param {Function} handler - 事件处理程序函数
     */
    removeHandler: function (element, type, handler) {
        if (element.removeEventListener) {//DOM2级事件处理程序
            element.removeEventListener(type, handler, false);
        } else if (element.detachEvent) {//IE事件处理程序
            element.detachEvent('on' + type, handler);
        } else {//DOM0级事件处理程序
            element['on' + type] = null;
        }
    },
    /**
     * 获取事件对象
     * @param event
     * @return {Event}
     */
    getEvent: function (event) {
        return event ? event : window.event;
    },
    /**
     * 获取事件目标
     * @param {Event} event
     * @return {string|EventTarget|Node|*|Object}
     */
    getTarget: function (event) {
        return event.target || event.srcElement;
    },
    /**
     * 取消事件默认行为
     * @param {Event} event
     */
    preventDefault: function (event) {
        if (event.preventDefault) {
            event.preventDefault();
        } else {
            event.returnValue = false;
        }
    },
    /**
     * 阻止事件冒泡
     * @param {Event} event
     */
    stopPropagation: function (event) {
        if (event.stopPropagation) {
            event.stopPropagation();
        } else {
            event.cancelBubble = true;
        }
    }
};
```
- `addHandler`
这个方法的职责是视情况分别使用 `DOM0` 级方法、`DOM2` 级方法或IE方法来添加事件。这个方法属于一个名叫 `EventUtil`的对象，本关卡用这个对象来处理浏览器间的差异。`addHandler()`方法接受3个参数：分别是要操作的元素、事件名称和事件处理程序函数
- `removeHandler`
与 `addHandler` 方法对应的方法是 `removeHandler`，它也接受与 `addHandler` 相同的参数。这个方法的职责是移除之前添加的事件处理程序，无论该事件处理程序是采取什么方式添加到元素中的，如果其他方法无效，默认采用 `DOM0` 级方法。

这两个方法首先都会检测掺入的元素中是否存在`DOM2`级方法。如果存在 `DOM2`级方法，则使用该方法。如果存在的是 IE 的方法，则采取第二种方法。**注意**，为额在 IE8 及更早版本中运行，此事的事件类型必须加上 “on” 前缀。最后一种可能是使用 `DOM0` 级方法（现代林蓝旗中，应该不会执行这里的代码）。此时，我们使用的方法是方括号语法来将属性名指定为时间程序，或者将属性设置为 `null`。可以像下面这样使用 `EventUtil` 对象:
```javascript
var btn = document.getElementById('myBtn');
var handler = function () {
    alert('Clicked');
};
EventUtil.addHandler(btn,'click',handler);
//这里省略了其他代码
EventUtil.removeHandler(btn,'click',handler);
```
`addHandler()`和`removeHandler()`没有考虑到所有浏览器问题，例如在 IE 中的作用于问题。不过，使用它们添加和移除事件处理程序还是足够了。此外还要注意， `DOM0` 级的浏览器几乎没有了，所以也不用太在乎这个问题。

- `getEvent`
这个方法返回对 `event` 对象的引用。考虑到 IE 中时间对象位置不同，可以使用这个方法来取得 `event` 对象，而不必担心制定时间处理程序的方法。在使用这个方法时，必须假设有一个时间对象传入到时间处理程序中，而且要把该变量传给这个方法，如下所示。
```javascript
btn.onclick = function (event) {
    event = EventUtil.getEvent(event);
};
```
在兼容 `DOM` 的浏览器中， `event` 变量只是简单地传入和返回。而在 IE 中，`event` 参数是未定义的(`undefined`)，因此就会返回 `window.event` 。将这一行代码添加到事件处理程序的开头，就可以确保随时都能使用 `event` 对象，而不必担心用户使用的是什么浏览器。
- `getTarget`
这个方法返回事件的目标。在这个方法的内部会检测 `event` 对象的 `target` 属性，如果存在则返回该属性的值；否则，返回 `srcElement` 属性的值。使用实例如下：
```javascript
btn.onclick = function (event) {
    event = EventUtil.getEvent(event);
    var target = EventUtil.getEvent(event);
};
```
- `preventDefault`
这个方法用于取消事件的默认行为。在传入 `event` 对象后，这个方法会检查是否存在 `preventDefault()` 方法，如果存在则调用该方法。如果 `preventDefault()` 方法不存在，则将 `returnValue` 设置为 `false`。
```javascript
var link = document.getElementById('myLink');
link.onclick = function (event) {
    event = EventUtil.getEvent(event);
    EventUtil.preventDefault(event);
};
```
以上代码可以确保在所有浏览器中单击该链接都不会打开另一个页面。首先，使用 `EventUtil.getEvent()` 取得 `event` 对象，然后将其传入到 `EventUtil.preventDefault()` 以取消默认行为
- `stopPropagation`
这个方法与 `preventDefault` 的实现方式累死。首先尝试使用 `DOM` 方法组织事件流，否则就使用 `cancelBubble` 属性。
```javascript
btn.onclick = function (event) {
    alert('Clicked');
    event = EventUtil.getEvent(event);
    EventUtil.stopPropagation(event);
};

document.body.onclick = function (event) {
    alert("Body clicked");
};
```
在此，首先使用 `EventUtil.getEvent()` 取得 `event` 对象，然后又将其传入到 `EventUtil.stopPropagation()` 。由于 IE 不支持事件捕获，因此这个方法在跨浏览器的情况下，只能用来阻止事件冒泡。