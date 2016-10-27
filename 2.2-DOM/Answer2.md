## 关卡详解

仔细想想，下面代码块会输出什么结果呢？前3个挑战相比前一章节，代码仅仅是多了换行，结果会有什么不一样呢？

```html
<!-- 挑战一 -->
<body>
<div id = "t">
    <span>aaa</span>
    <span>bbb</span>
    <span>ccc</span>
</div>
</body>
<script> 
    var d = document.getElementById("t");  
    document.writeln(d.firstChild.innerHTML);  // undefined
    document.writeln(d.lastChild.innerHTML);   // undefined   
</script>
```

```html
<!-- 挑战二 -->
<body name="ddd">
<div id = "t">
    <span>aaa</span>
    <span>bbb</span>
    <span>ccc</span>
 </div>
</body>
<script> 
    var d = document.getElementById("t");  
    document.writeln(d.childNodes[1].innerHTML); // aaa
    document.writeln(d.parentNode.getAttribute("name")); // ddd
</script>
```

```html
<!-- 挑战三 -->
<body name="ddd">
<div id = "t">
    <span>aaa</span>
    <span>bbb</span>
    <span>ccc</span>
</div>
</body>
<script> 
    var d = document.getElementById("t").childNodes[1];  
    document.writeln(d.nextSibling.innerHTML);      // undefined
    document.writeln(d.previousSibling.innerHTML);  // undefined
</script>
```

```html
<!-- 挑战四 -->
<body>
    <div id="t" class="content" style="background: red;" wife="sophie" onclick="alert('123');"></div>
</body>
<script> 
    var t = document.getElementById("t");    
    console.log(t.class);                   // undefined
    console.log(t.getAttribute("class"));   // "content"
    console.log(t.className);               // "content"
    console.log(t.getAttribute("className")); // null
    console.log(t.style);                   // [object MSStyleCSSPropertiesPrototype] 
    console.log(t.getAttribute("style"));   // "background: red;"
    console.log(t.style.background);        // "red"
    console.log(t.getAttribute("style.background")); // null
    console.log(t.wife);                    // undefined
    console.log(t.getAttribute("wife"));    // "sophie"
    console.log(t.onclick);                 // function onclick(event){ alert('123'); }
    console.log(t.getAttribute("onclick")); // "alert('123');"
</script>
```

