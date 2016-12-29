## 关卡详解


### 挑战一
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>test</title>
</head>
<body>
    <script type="text/javascript">

        var str = 'a';
        var charCode = str.charCodeAt();        

        for (var i = 0; i < 10; i++) {
            var m = charCode+i;
            for (var j = 0; j < 10; j++) {
                var n = charCode+j;
                    for (var k = 0; k < 10; k++) {
                        var l = charCode+k;
                        var varName = String.fromCharCode(m)+String.fromCharCode(n)+String.fromCharCode(l);
                        eval('var ' + varName + ' = Math.random();'); // eval仅作测试使用，真实代码严禁使用eval
                        console.log(varName + " = " +window[varName]);
                }            
            }
        }

        // 由于上面代码创建了1000个变量，悲催的 IE6 一直忙着CG，根本运行不到这里。
        location.href = 'http://www.baidu.com';

    </script>
</body>
</html>

```

### 挑战二
1. 使用快捷键 `F12` 打开 Firefox 浏览器的「开发者工具」。
2. 选择 `性能` 选项卡，点击 `开始记录性能` 按钮。
4. 打开一个网站，例如：[http://www.weibo.com](http://www.weibo.com)，当网页加载完成后，点击 `停止记录性能`，等待分析结果。
5. 然后在分析结果中寻找内存急速下降的部分，查看对应的瀑布图，可以从中找到 GC 的日志。

具体过程如下图所示：
![](http://qiniu.shijiajie.com/blog/javascript-lesson/2.6-GC&MemoryManagement/20161229171442.png)