## 关卡详解

仔细想想，下面代码块会输出什么结果呢？

```html
<!-- 挑战一 -->
<body>
<div id = "t"><span>aaa</span><span>bbb</span><span>ccc</span></div>
</body>
<script> 
    var d = document.getElementById("t");  
    document.writeln(d.firstChild.innerHTML);  // aaa
    document.writeln(d.lastChild.innerHTML);   // ccc   
</script>
```

```html
<!-- 挑战二 -->
<body name="ddd">
<div id = "t"><span>aaa</span><span>bbb</span><span>ccc</span></div>
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
<div id = "t"><span>aaa</span><span>bbb</span><span>ccc</span></div>
</body>
<script> 
    var d = document.getElementById("t").childNodes[1];  
    document.writeln(d.nextSibling.innerHTML);      // ccc
    document.writeln(d.previousSibling.innerHTML);  // aaa
</script>
```