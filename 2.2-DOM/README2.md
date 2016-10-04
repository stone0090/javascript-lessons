# DOM（文档对象模型）（下）

### Element 类型

除了 `Document` 类型之外，`Element` 类型就要算是 Web 编程中最常用的类型了。`Element` 类型用于表现 XML 或 HTML 元素，提供了对元素标签名、子节点及特性的访问。`Element` 节点具有以下特征：

- `nodeType` 的值为1；
- `nodeName` 的值为元素的标签名；
- `nodeValue` 的值为 `null`；
- `parentNode` 可能是 `Document` 或 `Element`；
- 其子节点可能是 `Element`、`Text`、`Comment`、`ProcessingInstruction`、`CDATASection` 或 `EntityReference`。

要访问元素的标签名，可以使用 `nodeName` 属性，也可以使用 `tagName` 属性；这两个属性会返回相同的值（使用后者主要是为了清晰起见）。以下面的元素为例：

```html
<div id="myDiv"></div>
```

可以像下面这样取得这个元素及其标签名：

```javascript
var div = document.getElementById("myDiv");
console.log(div.tagName); // "DIV"
console.log(div.tagName === div.nodeName); // true
```

这里的元素标签名是 `div`，它拥有一个值为 `"myDiv"` 的ID。可是，`div.tagName` 实际上输出的是 `"DIV"` 而非 `"div"`。在HTML中，标签名始终都以全部大写表示；而在 XML（有时候也包括 XHTML）中，标签名则始终会与源代码中的保持一致。假如你不确定自己的脚本将会在 HTML 还是 XML 文档中执行，最好是在比较之前将标签名转换为相同的大小写形式，如下面的例子所示：

```javascript
// 不能这样比较，很容易出错！
if (element.tagName == "div"){ 
    //在此执行某些操作
}

// 这样最好（适用于任何文档）
if (element.tagName.toLowerCase() == "div"){ 
    //在此执行某些操作
}
```

#### HTML 元素

所有 HTML 元素都由 `HTMLElement` 类型表示，不是直接通过这个类型，也是通过它的子类型来表示。`HTMLElement` 类型直接继承自 `Element` 并添加了一些属性。添加的这些属性分别对应于每个 HTML 元素中都存在的下列标准特性。

- `id`，元素在文档中的唯一标识符。
- `title`，有关元素的附加说明信息，一般通过工具提示条显示出来。
- `lang`，元素内容的语言代码，很少使用。
- `dir`，语言的方向，值为 `"ltr"`（left-to-right，从左至右）或 `"rtl"`（right-to-left，从右至左），也很少使用。
- `className`，与元素的 `class` 特性对应，即为元素指定的 CSS 类。没有将这个属性命名为 `class`，是因为 `class` 是 ECMAScript 的保留字。

上述这些属性都可以用来取得或修改相应的特性值。以下面的HTML元素为例：

```html
<div id="myDiv" class="bd" title="Body text" lang="en" dir="ltr"></div>
```

元素中指定的所有信息，都可以通过下列 JavaScript 代码取得：

```javascript
var div = document.getElementById("myDiv");
console.log(div.id);         // "myDiv""
console.log(div.className);  // "bd"
console.log(div.title);      // "Body text"
console.log(div.lang);       // "en"
console.log(div.dir);        // "ltr"
```

当然，像下面这样通过为每个属性赋予新的值，也可以修改对应的每个特性：

```javascript
div.id = "someOtherId";
div.className = "ft";
div.title = "Some other text";
div.lang = "fr";
div.dir ="rtl";
```

并不是对所有属性的修改都会在页面中直观地表现出来。对 `id` 或 `lang` 的修改对用户而言是透明不可见的（假设没有基于它们的值设置的 CSS 样式），而对 `title` 的修改则只会在鼠标移动到这个元素之上时才会显示出来。对 `dir` 的修改会在属性被重写的那一刻，立即影响页面中文本的左、右对齐方式。修改 `className` 时，如果新类关联了与此前不同的 CSS 样式，那么就会立即应用新的样式。

#### 取得特性

每个元素都有一或多个特性，这些特性的用途是给出相应元素或其内容的附加信息。操作特性的 DOM 方法主要有三个，分别是 `getAttribute()`、`setAttribute()` 和 `removeAttribute()`。这三个方法可以针对任何特性使用，包括那些以 `HTMLElement` 类型属性的形式定义的特性。来看下面的例子：

```javascript
var div = document.getElementById("myDiv");
console.log(div.getAttribute("id"));     // "myDiv"
console.log(div.getAttribute("class"));  // "bd"
console.log(div.getAttribute("title"));  // "Body text"
console.log(div.getAttribute("lang"));   // "en"
console.log(div.getAttribute("dir"));    // "ltr"
```

注意，传递给 `getAttribute()` 的特性名与实际的特性名相同。因此要想得到 `class` 特性值，应该传入 `"class"` 而不是 `"className"`，后者只有在通过对象属性访问特性时才用。如果给定名称的特性不存在，`getAttribute()` 返回 `null`。

通过 `getAttribute()` 方法也可以取得自定义特性（即标准 HTML 语言中没有的特性）的值，以下面的元素为例：

```html
<div id="myDiv" my_special_attribute="hello!"></div>
```

这个元素包含一个名为 `my_special_attribute` 的自定义特性，它的值是 `"hello!"`。可以像取得其他特性一样取得这个值，如下所示：

```javascript
var value = div.getAttribute("my_special_attribute");
```

不过，特性的名称是不区分大小写的，即 `"ID"` 和 `"id"` 代表的都是同一个特性。另外也要注意，根据 HTML5 规范，自定义特性应该加上 `data-` 前缀以便验证。

任何元素的所有特性，也都可以通过 DOM 元素本身的属性来访问。当然，`HTMLElement` 也会有5个属性与相应的特性一一对应。不过，只有公认的（非自定义的）特性才会以属性的形式添加到 DOM 对象中。以下面的元素为例：

```html
<div id="myDiv" align="left" my_special_attribute="hello!"></div>
```

因为 `id` 和 `align` 在 HTML 中是 `div` 的公认特性，因此该元素的 DOM 对象中也将存在对应的属性。不过，自定义特性 `my_special_attribute` 在 Safari、Opera、Chrome 及 Firefox 中是不存在的；但 IE 却会为自定义特性也创建属性，如下面的例子所示：

```javascript
console.log(div.id);                      // "myDiv"
console.log(div.my_special_attribute);    // undefined（IE除外）
console.log(div.align);                   // "left"
```

有两类特殊的特性，它们虽然有对应的属性名，但属性的值与通过 `getAttribute()` 返回的值并不相同。第一类特性就是 `style`，用于通过 CSS 为元素指定样式。在通过 `getAttribute()` 访问时，返回的 `style` 特性值中包含的是CSS文本，而通过属性来访问它则会返回一个对象。由于 `style` 属性是用于以编程方式访问元素样式的，因此并没有直接映射到 `style` 特性。

第二类与众不同的特性是 `onclick` 这样的事件处理程序。当在元素上使用时，`onclick` 特性中包含的是 JavaScript 代码，如果通过 `getAttribute()` 访问，则会返回相应代码的字符串。而在访问 `onclick` 属性时，则会返回一个 JavaScript 函数（如果未在元素中指定相应特性，则返回 null）。这是因为 `onclick` 及其他事件处理程序属性本身就应该被赋予函数值。

由于存在这些差别，在通过 JavaScript 以编程方式操作 DOM 时，开发人员经常不使用 `getAttribute()`，而是只使用对象的属性。只有在取得自定义特性值的情况下，才会使用 `getAttribute()` 方法。

#### 设置特性

与 `getAttribute()` 对应的方法是 `setAttribute()`，这个方法接受两个参数：要设置的特性名和值。如果特性已经存在，`setAttribute()` 会以指定的值替换现有的值；如果特性不存在，`setAttribute()` 则创建该属性并设置相应的值。来看下面的例子：

```javascript
div.setAttribute("id", "someOtherId");
div.setAttribute("class", "ft");
div.setAttribute("title", "Some other text");
div.setAttribute("lang","fr");
div.setAttribute("dir", "rtl");
```

通过 `setAttribute()` 方法既可以操作HTML特性也可以操作自定义特性。通过这个方法设置的特性名会被统一转换为小写形式，即 `"ID"` 最终会变成 `"id"`。

因为所有特性都是属性，所以直接给属性赋值可以设置特性的值，如下所示。

```javascript
div.id = "someOtherId";
div.align = "left";
```

不过，像下面这样为 DOM 元素添加一个自定义的属性，该属性不会自动成为元素的特性。

```javascript
div.mycolor = "red";
console.log(div.mycolor); // "red"
console.log(div.getAttribute("mycolor")); // null（IE除外）
```

这个例子添加了一个名为 `mycolor` 的属性并将它的值设置为 `"red"`。在大多数浏览器中，这个属性都不会自动变成元素的特性，因此想通过 `getAttribute()` 取得同名特性的值，结果会返回 `null`。可是，自定义属性在IE中会被当作元素的特性，反之亦然。

要介绍的最后一个方法是 `removeAttribute()`，这个方法用于彻底删除元素的特性。调用这个方法不仅会清除特性的值，而且也会从元素中完全删除特性，如下所示：

```javascript
div.removeAttribute("class");
```

这个方法并不常用，但在序列化 DOM 元素时，可以通过它来确切地指定要包含哪些特性。

#### 创建元素

使用 `document.createElement()` 方法可以创建新元素。这个方法只接受一个参数，即要创建元素的标签名。这个标签名在 HTML 文档中不区分大小写。例如，使用下面的代码可以创建一个 `div` 元素。

```javascript
var div = document.createElement("div");
```

在使用 `createElement()` 方法创建新元素的同时，也为新元素设置了 `ownerDocuemnt` 属性。此时，还可以操作元素的特性，为它添加更多子节点，以及执行其他操作。来看下面的例子。

```javascript
div.id = "myNewDiv";
div.className = "box";
```

在新元素上设置这些特性只是给它们赋予了相应的信息。由于新元素尚未被添加到文档树中，因此设置这些特性不会影响浏览器的显示。要把新元素添加到文档树，可以使用 `appendChild()`、`insertBefore()` 或 `replaceChild()` 方法。下面的代码会把新创建的元素添加到文档的 `<body>` 元素中。

```javascript
document.body.appendChild(div);
```

一旦将元素添加到文档树中，浏览器就会立即呈现该元素。此后，对这个元素所作的任何修改都会实时反映在浏览器中。

#### 元素的子节点   

元素可以有任意数目的子节点和后代节点，因为元素可以是其他元素的子节点。元素的 `childNodes` 属性中包含了它的所有子节点，这些子节点有可能是元素、文本节点、注释或处理指令。不同浏览器在看待这些节点方面存在显著的不同，以下面的代码为例。

```html
<ul id="myList">
    <li>Item 1</li>
    <li>Item 2</li>
    <li>Item 3</li>
</ul>
```

如果是IE来解析这些代码，那么 `<ul>` 元素会有3个子节点，分别是3个 `<li>` 元素。但如果是在其他浏览器中，`<ul>` 元素都会有7个元素，包括3个 `<li>` 元素和4个文本节点（表示 `<li>` 元素之间的空白符）。如果像下面这样将元素间的空白符删除，那么所有浏览器都会返回相同数目的子节点。

```html
<ul id="myList"><li>Item 1</li><li>Item 2</li><li>Item 3</li></ul>
```

对于这段代码，`<ul>` 元素在任何浏览器中都会包含3个子节点。如果需要通过 `childNodes` 属性遍历子节点，那么一定不要忘记浏览器间的这一差别。这意味着在执行某项操作以前，通常都要先检查一下 `nodeTpye` 属性，如下面的例子所示。

```javascript
for (var i=0, len=element.childNodes.length; i < len; i++){
    if (element.childNodes[i].nodeType == 1){
        //执行某些操作
    }
}
```

这个例子会循环遍历特定元素的每一个子节点，然后只在子节点的 `nodeType` 等于1（表示是元素节点）的情况下，才会执行某些操作。

如果想通过某个特定的标签名取得子节点或后代节点该怎么办呢？实际上，元素也支持 `getElementsByTagName()` 方法。在通过元素调用这个方法时，除了搜索起点是当前元素之外，其他方面都跟通过 `document` 调用这个方法相同，因此结果只会返回当前元素的后代。例如，要想取得前面 `<ul>` 元素中包含的所有 `<li>` 元素，可以使用下列代码。

```javascript
var ul = document.getElementById("myList");
var items = ul.getElementsByTagName("li");
```

要注意的是，这里 `<ul>` 的后代中只包含直接子元素。不过，如果它包含更多层次的后代元素，那么各个层次中包含的 `<li>` 元素也都会返回。

### Text 类型

文本节点由 `Text` 类型表示，包含的是可以照字面解释的纯文本内容。纯文本中可以包含转义后的 HTML 字符，但不能包含 HTML 代码。`Text` 节点具有以下特征：

- `nodeType` 的值为3；
- `nodeName` 的值为 `"#text"`；
- `nodeValue` 的值为节点所包含的文本；
- `parentNode` 是一个 `Element`；
- 不支持（没有）子节点。

可以通过 `nodeValue` 属性或 `data` 属性访问 `Text` 节点中包含的文本，这两个属性中包含的值相同。对 `nodeValue `的修改也会通过 `data` 反映出来，反之亦然。使用下列方法可以操作节点中的文本。

- `appendData(*text*)`：将 `*text*` 添加到节点的末尾。
- `deleteData(*offset*, *count*)`：从 `*offset*` 指定的位置开始删除 `*count*` 个字符。
- `insertData(*offset, text*)`：在 `*offset*` 指定的位置插入 `*text*`。
- `replaceData(*offset, count, text*)`：用 `*text*` 替换从 `*offset*` 指定的位置开始到 `*offset*+*count*` 为止处的文本。
- `splitText(*offset*)`：从 `*offset*` 指定的位置将当前文本节点分成两个文本节点。
- `substringData(*offset, count*)`：提取从 `*offset*` 指定的位置开始到 `*offset+count*` 为止处的字符串。

除了这些方法之外，文本节点还有一个 `length` 属性，保存着节点中字符的数目。而且，`nodeValue.length` 和 `data.length` 中也保存着同样的值。

在默认情况下，每个可以包含内容的元素最多只能有一个文本节点，而且必须确实有内容存在。来看几个例子。

```html
<!-- 没有内容，也就没有文本节点 -->
<div></div>

<!-- 有空格，因而有一个文本节点 -->
<div> </div>

<!-- 有内容，因而有一个文本节点 -->
<div>Hello World!</div>
```

上面代码给出的第一个 `<div>` 元素没有内容，因此也就不存在文本节点。开始与结束标签之间只要存在内容，就会创建一个文本节点。因此，第二个 `<div>` 元素中虽然只包含一个空格，但仍然有一个文本子节点；文本节点的 `nodeValue` 值是一个空格。第三个 `div` 也有一个文本节点，其 `nodeValue` 的值为 `"Hello World!"`。可以使用以下代码来访问这些文本子节点。

```javascript
var textNode = div.firstChild;  // 或者div.childNodes[0]
```

在取得了文本节点的引用后，就可以像下面这样来修改它了。

```javascript
div.firstChild.nodeValue = "Some other message";
```

如果这个文本节点当前存在于文档树中，那么修改文本节点的结果就会立即得到反映。另外，在修改文本节点时还要注意，此时的字符串会经过 HTML（或XML，取决于文档类型）编码。换句话说，小于号、大于号或引号都会像下面的例子一样被转义。


```javascript
// 输出结果是"Some &lt;strong&gt;other&lt;/strong&gt; message"
div.firstChild.nodeValue = "Some <strong>other</strong> message";
```

应该说，这是在向 DOM 文档中插入文本之前，先对其进行 HTML 编码的一种有效方式。

> 在IE8、Firefox、Safari、Chrome和Opera中，可以通过脚本访问 `Text` 类型的构造函数和原型。

#### 创建文本节点

可以使用 `document.createTextNode()` 创建新文本节点，这个方法接受一个参数——要插入节点中的文本。与设置已有文本节点的值一样，作为参数的文本也将按照 HTML 或 XML 的格式进行编码。

```javascript
var textNode = document.createTextNode("<strong>Hello</strong> world!");
```

在创建新文本节点的同时，也会为其设置 `ownerDocument` 属性。不过，除非把新节点添加到文档树中已经存在的节点中，否则我们不会在浏览器窗口中看到新节点。下面的代码会创建一个 `<div>` 元素并向其中添加一条消息。

```javascript
var element = document.createElement("div");
element.className = "message";

var textNode = document.createTextNode("Hello world!");
element.appendChild(textNode);

document.body.appendChild(element);
```

这个例子创建了一个新 `<div>` 元素并为它指定了值为 `"message"` 的 `class` 特性。然后，又创建了一个文本节点，并将其添加到前面创建的元素中。最后一步，就是将这个元素添加到了文档的 `<body>` 元素中，这样就可以在浏览器中看到新创建的元素和文本节点了。

一般情况下，每个元素只有一个文本子节点。不过，在某些情况下也可能包含多个文本子节点，如下面的例子所示。

```javascript
var element = document.createElement("div");
element.className = "message";

var textNode = document.createTextNode("Hello world!");
element.appendChild(textNode);

var anotherTextNode = document.createTextNode("Yippee!");
element.appendChild(anotherTextNode);

document.body.appendChild(element);
```

如果两个文本节点是相邻的同胞节点，那么这两个节点中的文本就会连起来显示，中间不会有空格。

#### 规范化文本节点

DOM 文档中存在相邻的同胞文本节点很容易导致混乱，因为分不清哪个文本节点表示哪个字符串。另外，DOM 文档中出现相邻文本节点的情况也不在少数，于是就催生了一个能够将相邻文本节点合并的方法。这个方法是由 `Node` 类型定义的（因而在所有节点类型中都存在），名叫 `normalize()`。如果在一个包含两个或多个文本节点的父元素上调用 `normalize()` 方法，则会将所有文本节点合并成一个节点，结果节点的 `nodeValue` 等于将合并前每个文本节点的 `nodeValue` 值拼接起来的值。来看一个例子。

```javascript
var element = document.createElement("div");
element.className = "message";

var textNode = document.createTextNode("Hello world!");
element.appendChild(textNode);

var anotherTextNode = document.createTextNode("Yippee!");
element.appendChild(anotherTextNode);

document.body.appendChild(element);
console.log(element.childNodes.length);    // 2

element.normalize();
console.log(element.childNodes.length);    // 1
console.log(element.firstChild.nodeValue); // "Hello world!Yippee!"
```

浏览器在解析文档时永远不会创建相邻的文本节点。这种情况只会作为执行DOM操作的结果出现。

#### 分割文本节点

`Text` 类型提供了一个作用与 `normalize()` 相反的方法 `splitText()`。这个方法会将一个文本节点分成两个文本节点，即按照指定的位置分割 `nodeValue` 值。原来的文本节点将包含从开始到指定位置之前的内容，新文本节点将包含剩下的文本。这个方法会返回一个新文本节点，该节点与原节点的 `parentNode` 相同。

### Comment 类型

注释在 DOM 中是通过 `Comment` 类型来表示的。`Comment` 节点具有下列特征：

- `nodeType` 的值为8；
- `nodeName` 的值为 `"#comment"`；
- `nodeValue` 的值是注释的内容；
- `parentNode` 可能是 `Document` 或 `Element`；
- 不支持（没有）子节点。

`Comment` 类型与 `Text` 类型继承自相同的基类，因此它拥有除`splitText()` 之外的所有字符串操作方法。与 `Text` 类型相似，也可以通过 `nodeValue` 或 `data` 属性来取得注释的内容。

注释节点可以通过其父节点来访问，以下面的代码为例。

```html
<div id="myDiv"><!--A comment --></div>
```

在此，注释节点是 `<div>` 元素的一个子节点，因此可以通过下面的代码来访问它。

```javascript
var div = document.getElementById("myDiv");
var comment = div.firstChild;
console.log(comment.data);    // "A comment"
```

另外，使用 `document.createComment()` 并为其传递注释文本也可以创建注释节点，如下面的例子所示。

```javascript
var comment = document.createComment("A comment ");
```

显然，开发人员很少会创建和访问注释节点，因为注释节点对算法鲜有影响。此外，浏览器也不会识别位于 `</html>` 标签后面的注释。如果要访问注释节点，一定要保证它们位于 `<html>` 和 `</html>` 之间。

### Attr 类型

元素的特性在 DOM 中以 `Attr` 类型来表示。在所有浏览器中（包括IE8），都可以访问 `Attr` 类型的构造函数和原型。从技术角度讲，特性就是存在于元素的 `attributes` 属性中的节点。特性节点具有下列特征：

- `nodeType` 的值为11；
- `nodeName` 的值是特性的名称；
- `nodeValue` 的值是特性的值；
- `parentNode` 的值为 `null`；
- 在 HTML 中不支持（没有）子节点；
- 在 XML 中子节点可以是 `Text `或 `EntityReference`。

尽管它们也是节点，但特性却不被认为是DOM文档树的一部分。开发人员最常使用的是 `getAttribute()`、`setAttribute()` 和 `remveAttribute()` 方法，很少直接引用特性节点。

`Attr` 对象有3个属性：`name`、`value` 和 `specified`。其中，`name` 是特性名称（与 `nodeName` 的值相同），`value` 是特性的值（与 `nodeValue` 的值相同），而 `specified` 是一个布尔值，用以区别特性是在代码中指定的，还是默认的。

使用 `document.createAttribute()` 并传入特性的名称可以创建新的特性节点。例如，要为元素添加 `align` 特性，可以使用下列代码：

```javascript
var attr = document.createAttribute("align");
attr.value = "left";
element.setAttributeNode(attr);
console.log(element.attributes["align"].value);       // "left"
console.log(element.getAttributeNode("align").value); // "left"
console.log(element.getAttribute("align"));           // "left"
```

添加特性之后，可以通过下列任何方式访问该特性：`attributes` 属性、`getAttributeNode()` 方法以及 `getAttribute()` 方法。其中，`attributes` 和 `getAttributeNode()` 都会返回对应特性的 `Attr` 节点，而 `getAttribute()` 则只返回特性的值。

## DOM 操作

很多时候，DOM 操作都比较简明，因此用 JavaScript 生成那些通常原本是用 HTML 代码生成的内容并不麻烦。不过，也有一些时候，操作 DOM 并不像表面上看起来那么简单。由于浏览器中充斥着隐藏的陷阱和不兼容问题，用 JavaScript 代码处理 DOM 的某些部分要比处理其他部分更复杂一些。

### 动态脚本

使用 `<script>` 元素可以向页面中插入 JavaScript 代码，一种方式是通过其 `src` 特性包含外部文件，另一种方式就是用这个元素本身来包含代码。而这一节要讨论的动态脚本，指的是在页面加载时不存在，但将来的某一时刻通过修改 DOM 动态添加的脚本。跟操作 HTML 元素一样，创建动态脚本也有两种方式：插入外部文件和直接插入 JavaScript 代码。

动态加载的外部 JavaScript 文件能够立即运行，比如下面的 `<script>` 元素：

```html
<script type="text/javascript" src="client.js"></script>
```

创建这个 DOM 节点的代码如下所示：

```javascript
function loadScript(url){
    var script = document.createElement("script");
    script.type = "text/javascript";
    script.src = url;
    document.body.appendChild(script);
}
```

下面是调用这个函数的示例：

```javascript
loadScript("client.js");
```

另一种指定 JavaScript 代码的方式是行内方式，如下面的例子所示：

```html
<script type="text/javascript">
    function sayHi(){
        alert("hi");
    }
</script>
```

从逻辑上讲，下面操作的 DOM 代码是有效的：

```javascript
var script = document.createElement("script");
script.type = "text/javascript";
script.appendChild(document.createTextNode("function sayHi(){alert('hi');}"));
document.body.appendChild(script);
```

在 Firefox、Safari、Chrome 和 Opera 中，这些 DOM 代码可以正常运行。但在 IE 中，则会导致错误。IE 将 `<script>` 视为一个特殊的元素，不允许 DOM 访问其子节点。不过，可以使用 `<script>` 元素的`text` 属性来指定 JavaScript 代码，像下面的例子这样：

```javascript
var script = document.createElement("script");
script.type = "text/javascript";
script.text = "function sayHi(){alert('hi');}";
document.body.appendChild(script);
```
整个过程可以用以下函数来表示：

```javascript
function loadScriptString(code){
    var script = document.createElement("script");
    script.type = "text/javascript";
    try {
        script.appendChild(document.createTextNode(code));
    } catch (ex){
        script.text = code;
    }
    document.body.appendChild(script);
}
```

下面是调用这个函数的示例：

```javascript
loadScriptString("function sayHi(){alert('hi');}");
```

以这种方式加载的代码会在全局作用域中执行，而且当脚本执行后将立即可用。实际上，这样执行代码与在全局作用域中把相同的字符串传递给 `eval()` 是一样的。

### 动态样式

能够把 CSS 样式包含到 HTML 页面中的元素有两个。其中，`<link>` 元素用于包含来自外部的文件，而 `<style>` 元素用于指定嵌入的样式。与动态脚本类似，所谓动态样式是指在页面刚加载时不存在的样式；动态样式是在页面加载完成后动态添加到页面中的。

我们以下面这个典型的 `<link>` 元素为例：

```html
<link rel="stylesheet" type="text/css" href="styles.css">
```

使用 DOM 代码可以很容易地动态创建出这个元素：

```javascript
function loadStyles(url){
    var link = document.createElement("link");
    link.rel = "stylesheet";
    link.type = "text/css";
    link.href = url;
    var head = document.getElementsByTagName("head")[0];
    head.appendChild(link);
}
```

以上代码在所有主流浏览器中都可以正常运行。需要注意的是，必须将 `<link>` 元素添加到 `<head>` 而不是 `<body>` 元素，才能保证在所有浏览器中的行为一致。调用函数的代码如下所示：

```javascript
loadStyles("styles.css");
```

加载外部样式文件的过程是异步的，也就是加载样式的过程没有固定的次序。一般来说，知不知道样式已经加载完成并不重要。

另一种定义样式的方式是使用 `<style>` 元素来包含嵌入式 CSS，如下所示：

```html
<style type="text/css">
body { background-color: red; }
</style>
```

按照相同的逻辑，下列 DOM 代码应该是有效的：

```javascript
var style = document.createElement("style");
style.type = "text/css";
style.appendChild(document.createTextNode("body{background-color:red}")); 
var head = document.getElementsByTagName("head")[0];
head.appendChild(style);
```

以上代码可以在 Firefox、Safari、Chrome 和 Opera 中运行，在 IE 中则会报错。IE将 `<style>` 视为一个特殊的、与 `<script>` 类似的节点，不允许访问其子节点。事实上，IE 此时抛出的错误与向 `<script>` 元素添加子节点时抛出的错误相同。解决 IE 中这个问题的办法，就是访问元素的 `styleSheet` 属性，该属性又有一个 `cssText` 属性，可以接受 CSS 代码），如下面的例子所示。

```javascript
function loadStyleString(css){
    var style = document.createElement("style");
    style.type = "text/css";
    try{
        style.appendChild(document.createTextNode(css));
    } catch (ex){
        style.styleSheet.cssText = css;
    }
    var head = document.getElementsByTagName("head")[0];
    head.appendChild(style);
}
```

调用这个函数的示例如下：

```javascript
loadStyleString("body{background-color:red}");
```

这种方式会实时地向页面中添加样式，因此能够马上看到变化。

### 使用 NodeList

理解 `NodeList` 及其“近亲” `NamedNodeMap` 和 `HTMLCollection`，是从整体上透彻理解 DOM 的关键所在。这三个集合都是“动态的”；换句话说，每当文档结构发生变化时，它们都会得到更新。因此，它们始终都会保存着最新、最准确的信息。从本质上说，所有 `NodeList` 对象都是在访问 DOM 文档时实时运行的查询。例如，下列代码会导致无限循环：

```javascript
var divs = document.getElementsByTagName("div"),
    i, 
    div;

for (i=0; i < divs.length; i++){
    div = document.createElement("div");
    document.body.appendChild(div);
}
```

第一行代码会取得文档中所有 `<div>` 元素的 `HTMLCollection`。由于这个集合是“动态的”，因此只要有新 `<div>` 元素被添加到页面中，这个元素也会被添加到该集合中。浏览器不会将创建的所有集合都保存在一个列表中，而是在下一次访问集合时再更新集合。结果，在遇到上例中所示的循环代码时，就会导致一个有趣的问题。每次循环都要对条件 `i < divs.length` 求值，意味着会运行取得所有 `<div>` 元素的查询。考虑到循环体每次都会创建一个新 `<div>` 元素并将其添加到文档中，因此 `divs.length` 的值在每次循环后都会递增。既然`i`和 `divs.length` 每次都会同时递增，结果它们的值永远也不会相等。

如果想要迭代一个 `NodeList`，最好是使用 `length` 属性初始化第二个变量，然后将迭代器与该变量进行比较，如下面的例子所示：

```javascript
var divs = document.getElementsByTagName("div"),
    i,
    len,
    div;

for (i=0, len=divs.length; i < len; i++){
    div = document.createElement("div");
    document.body.appendChild(div);
}
```

这个例子中初始化了第二个变量 `len`。由于 `len` 中保存着对 `divs.length` 在循环开始时的一个快照，因此就会避免上一个例子中出现的无限循环问题。在本章演示迭代 `NodeList` 对象的例子中，使用的都是这种更为保险的方式。

一般来说，应该尽量减少访问 `NodeList` 的次数。因为每次访问 `NodeList`，都会运行一次基于文档的查询。所以，可以考虑将从 `NodeList` 中取得的值缓存起来。

## 小结

DOM 是语言中立的 API，用于访问和操作 HTML 和 XML 文档。DOM1 级将 HTML 和 XML 文档形象地看作一个层次化的节点树，可以使用 JavaScript 来操作这个节点树，进而改变底层文档的外观和结构。

DOM 由各种节点构成，简要总结如下。

- 最基本的节点类型是 `Node`，用于抽象地表示文档中一个独立的部分；所有其他类型都继承自 `Node`。
- `Document` 类型表示整个文档，是一组分层节点的根节点。在 JavaScript 中，`document` 对象是 `Document` 的一个实例。使用 `document` 对象，有很多种方式可以查询和取得节点。
- `Element` 节点表示文档中的所有 HTML 或 XML 元素，可以用来操作这些元素的内容和特性。
- 另外还有一些节点类型，分别表示文本内容、注释、文档类型、CDATA 区域和文档片段。

访问 DOM 的操作在多数情况下都很直观，不过在处理 `<script>` 和 `<style>` 元素时还是存在一些复杂性。由于这两个元素分别包含脚本和样式信息，因此浏览器通常会将它们与其他元素区别对待。这些区别导致了在针对这些元素使用 `innerHTML` 时，以及在创建新元素时的一些问题。

理解 DOM 的关键，就是理解 DOM 对性能的影响。DOM 操作往往是 JavaScript 程序中开销最大的部分，而因访问 `NodeList` 导致的问题为最多。`NodeList` 对象都是“动态的”，这就意味着每次访问 `NodeList` 对象，都会运行一次查询。有鉴于此，最好的办法就是尽量减少 DOM 操作。

## 更多

> 关注微信公众号「劼哥舍」回复「答案」，获取关卡详解。  
> 关注 [https://github.com/stone0090/javascript-lessons](https://github.com/stone0090/javascript-lessons)，获取最新动态。