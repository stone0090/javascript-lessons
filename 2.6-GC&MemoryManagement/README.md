# 垃圾回收和内存管理

JavaScript 具有自动垃圾收集机制（GC：Garbage Collecation），也就是说，执行环境会负责管理代码执行过程中使用的内存。而在 C 和 C++ 之类的语言中，开发人员的一项基本任务就是手工跟踪内存的使用情况，这是造成许多问题的一个根源。

在编写 JavaScript 程序时，开发人员不用再关心内存使用问题，所需内存的分配以及无用内存的回收完全实现了自动管理。这种垃圾收集机制的原理其实很简单：**找出那些不再继续使用的变量，然后释放其占用的内存。**为此，垃圾收集器会按照固定的时间间隔（或代码执行中预定的收集时间），周期性地执行这一操作。

正因为垃圾回收器的存在，许多人认为 JavaScript 不用太关心内存管理的问题，但如果不了解 JavaScript 的内存管理机制，我们同样非常容易成内存泄漏（内存无法被回收）的情况。

## 垃圾回收机制

### 内存的分配场景

```javascript
// 1.对象
new Object(); 
new MyConstructor(); 
{ a: 4, b: 5 } 
Object.create(); 
```

```javascript
// 2.数组 
new Array(); 
[ 1, 2, 3, 4 ]; 
```

```javascript
// 3.字符串，JavaScript 的字符串和 .NET 一样，使用资源池和 copy on write 方式管理字符串。
new String("hello hyddd"); 
"<p>" + e.innerHTML + "</p>" 
```

```javascript
// 4.函数
var x = function () { ... } 
new Function(code); 
```

```javascript
// 5.闭包 
function outer(name) {
     var x = name; 
     return function inner() { 
        return "Hi, " + name; 
     } 
 }
```

### 内存的生命周期

下面我们来分析一下函数中局部变量的正常生命周期。

- 内存分配：局部变量只在函数执行的过程中存在。而在这个过程中，会为局部变量在栈（或堆）内存上分配相应的空间，以便存储它们的值。
- 内存使用：然后在函数中使用这些变量，直至函数执行结束。
- 内存回收：此时，局部变量就没有存在的必要了，因此可以释放它们的内存以供将来使用。

通常，很容易判断变量是否还有存在的必要，但并非所有情况下都这么容易就能得出结论（例如：使用闭包的时）。垃圾收集器必须跟踪哪个变量有用哪个变量没用，对于不再有用的变量打上标记，以备将来收回其占用的内存。用于标识无用变量的策略可能会因实现而异，但具体到浏览器中的实现，则通常有两个策略：**标记清除** 和 **引用计数**。

### 标记清除

JavaScript 中最常用的垃圾收集方式是 **标记清除**（mark-and-sweep）。当变量进入环境（例如，在函数中声明一个变量）时，就将这个变量标记为“进入环境”。从逻辑上讲，永远不能释放进入环境的变量所占用的内存，因为只要执行流进入相应的环境，就可能会用到它们。而当变量离开环境时，则将其标记为“离开环境”。

```javascript
function test(){ 
    var a = 10 ; // 被标记 ，进入环境 
    var b = 20 ; // 被标记 ，进入环境 
} 
test(); // 执行完毕 之后 a、b又被标离开环境，被回收。
```

垃圾回收器在运行的时候会给存储在内存中的所有变量都加上标记（当然，可以使用任何标记方式）。然后，它会去掉环境中的变量以及被环境中的变量引用的变量的标记（例如，闭包）。而在此之后再被加上标记的变量将被视为准备删除的变量，原因是环境中的变量已经无法访问到这些变量了。最后，垃圾回收器完成内存清除工作，销毁那些带标记的值并回收它们所占用的内存空间。

这种方式的主要缺点就是如果某些对象被清理后，内存是不连续的，那么就算内存占用率不高，例如只有50%，但是由于内存空隙太多，后来的大对象甚至无法存储到内存之中。一般的处理方式都是在垃圾回收后进行整理操作，这种方法也叫 **标记整理**，整理的过程就是将不连续的内存向一端复制，使不连续的内存连续起来。

目前，IE9+、Firefox、Opera、Chrome 和 Safari 的 JavaScript 实现使用的都是 **标记清除** 式的垃圾收集策略（或类似的策略），只不过垃圾收集的时间间隔互有不同。

### 引用计数

另一种不太常见的垃圾收集策略叫做 **引用计数**（reference counting）。引用计数的含义是跟踪记录每个值被引用的次数。当声明了一个变量并将一个引用类型值赋给该变量时，则这个值的引用次数就是1。如果同一个值又被赋给另一个变量，则该值的引用次数加1。相反，如果包含对这个值引用的变量又取得了另外一个值，则这个值的引用次数减1。当这个值的引用次数变成0时，则说明没有办法再访问这个值了，因而就可以将其占用的内存空间回收回来。这样，当垃圾收集器下次再运行时，它就会释放那些引用次数为零的值所占用的内存。

```javascript
function test(){ 
    var a = {} ; // a的引用次数为0 
    var b = a ; // a的引用次数加1，为1 
    var c = a; // a的引用次数再加1，为2 
    var b = {}; // a的引用次数减1，为1 
}
```

早期很多浏览器使用引用计数策略，但很快它就遇到了一个严重的问题：**循环引用**。循环引用指的是对象 A 中包含一个指向对象 B 的指针，而对象 B 中也包含一个指向对象 A 的引用。请看下面这个例子：

```javascript
function problem(){
    var objectA = new Object();
    var objectB = new Object();

    objectA.someOtherObject = objectB;
    objectB.anotherObject = objectA;
}
```
在这个例子中，objectA 和 objectB 通过各自的属性相互引用；也就是说，这两个对象的引用次数都是2。在采用 **标记清除** 策略的实现中，由于函数执行之后，这两个对象都离开了作用域，因此这种相互引用不是个问题。但在采用 **引用计数** 策略的实现中，当函数执行完毕后，objectA 和 objectB 还将继续存在，因为它们的引用次数永远不会是0。假如这个函数被重复多次调用，就会导致大量内存得不到回收。为此，新一代浏览器都放弃了引用计数方式，转而采用标记清除来实现其垃圾收集机制。可是，引用计数导致的麻烦并未就此终结。

我们知道，IE 中有一部分对象并不是原生 JavaScript 对象。例如，其 BOM 和 DOM 中的对象就是使用 C++ 以 COM（Component Object Model，组件对象模型）对象的形式实现的，而 COM 对象的垃圾收集机制采用的就是引用计数策略。因此，即使 IE 的 JavaScript 引擎是使用标记清除策略来实现的，但 JavaScript 访问的 COM 对象依然是基于引用计数策略的。换句话说，只要在 IE 中涉及 COM 对象，就会存在循环引用的问题。下面这个简单的例子，展示了使用 COM 对象导致的循环引用问题：

```javascript
var element = document.getElementById("some_element");
var myObject = new Object();
myObject.element = element;
element.someObject = myObject;
```

这个例子在一个 DOM 元素（element）与一个原生 JavaScript 对象（myObject）之间创建了循环引用。其中，变量 myObject 有一个名为 element 的属性指向 element 对象；而变量 element 也有一个属性名叫 someObject 回指 myObject。由于存在这个循环引用，即使将例子中的 DOM 从页面中移除，它也永远不会被回收。

为了避免类似这样的循环引用问题，最好是在不使用它们的时候手工断开原生 JavaScript 对象与 DOM 元素之间的连接。例如，可以使用下面的代码消除前面例子创建的循环引用：

```javascript
myObject.element = null;
element.someObject = null;
```

将变量设置为 `null` 意味着切断变量与它此前引用的值之间的连接。当垃圾收集器下次运行时，就会删除这些值并回收它们占用的内存。

为了解决上述问题，IE9 把 BOM 和 DOM 对象都转换成了真正的 JavaScript 对象。这样，就避免了两种垃圾收集算法并存导致的问题，也消除了常见的内存泄漏现象。

### IE6 的性能问题

IE6 的垃圾回收是根据内存分配量运行的，当环境中存在256个变量、4096个对象、64k的字符串任意一种情况的时候就会触发垃圾回收器工作，看起来很科学，不用按一段时间就调用一次，有时候会没必要，这样按需调用不是很好吗？但是如果环境中就是有这么多变量等一直存在，现在脚本如此复杂，那么垃圾回收器会一直工作，这样浏览器就没法儿玩儿了。

微软在 IE7 中做了调整，触发条件不再是固定的，而是动态修改的，初始值和 IE6 相同，如果垃圾回收器回收的内存分配量低于程序占用内存的15%，说明大部分内存不可被回收，设的垃圾回收触发条件过于敏感，这时候把临界条件翻倍，如果回收的内存高于85%，说明大部分内存早就该清理了，这时候则将各种临界值重置回默认值。这一看似简单的调整，极大地提升了 IE7 在运行包含大量 JavaScript 的页面时的性能。

### 编码注意 - 解除引用

使用具备垃圾收集机制的语言编写程序，开发人员一般不必操心内存管理的问题。但是，JavaScript 在进行内存管理及垃圾收集时面临的问题还是有点与众不同。其中最主要的一个问题，就是分配给 Web 浏览器的可用内存数量通常要比分配给桌面应用程序的少。这样做的目的主要是出于安全方面的考虑，目的是防止运行 JavaScript 的网页耗尽全部系统内存而导致系统崩溃。内存限制问题不仅会影响给变量分配内存，同时还会影响调用栈以及在一个线程中能够同时执行的语句数量。

因此，确保占用最少的内存可以让页面获得更好的性能。而优化内存占用的最佳方式，就是为执行中的代码只保存必要的数据。一旦数据不再有用，最好通过将其值设置为 `null` 来释放其引用——这个做法叫做 **解除引用**（dereferencing）。这一做法适用于大多数全局变量和全局对象的属性。局部变量会在它们离开执行环境时自动被解除引用，如下面这个例子所示：

```javascript
function createPerson(name){
    var localPerson = new Object();
    localPerson.name = name;
    return localPerson;
}

var globalPerson = createPerson("Nicholas");

// 手工解除globalPerson的引用
globalPerson = null;
```

由于局部变量 `localPerson` 在 `createPerson()` 函数执行完毕后就离开了其执行环境，因此无需我们显式地去为它解除引用。但是对于全局变量 `globalPerson` 而言，则需要我们在不使用它的时候手工为它解除引用，这也正是上面例子中最后一行代码的目的。

不过，解除一个值的引用并不意味着自动回收该值所占用的内存。解除引用的真正作用是让值脱离执行环境，以便垃圾收集器下次运行时将其回收。

## 垃圾回收的优化策略

和其他语言一样，JavaScript 的垃圾回收策略也无法避免一个问题：垃圾回收时，会停止响应其他操作，这是为了安全考虑。而 JavaScript 的垃圾回收在 100ms 甚至以上，对一般的应用还好，但对于 JavaScript 游戏和动画，这种对连贯性要求比较高的应用，就麻烦了。这就是新引擎需要优化的点：避免垃圾回收造成的长时间停止响应。

David 大叔主要介绍了2个优化方案，而这也是最主要的2个优化方案了： 

### 分代回收（Generation GC） 

这个和 Java 回收策略思想是一致的。目的是通过区分「临时」与「持久」对象；多回收「临时对象区」（young generation），少回收「持久对象区」（tenured generation），减少每次需遍历的对象，从而减少每次GC的耗时。**Chrome 浏览器所使用的 V8 引擎就是采用的分代回收策略。**如图： 

![](http://qiniu.shijiajie.com/blog/javascript-lesson/2.6-GC&MemoryManagement/WechatIMG124.jpg)

### 增量回收（Incremental GC）

这个方案的思想很简单，就是「每次处理一点，下次再处理一点，如此类推」。这种方案，虽然耗时短，但中断较多，带来了上下文切换频繁的问题。**Firefox 浏览器所使用的  JavaScript 引擎就是采用的增量回收策略。**如图： 

![](http://qiniu.shijiajie.com/blog/javascript-lesson/2.6-GC&MemoryManagement/WechatIMG125.jpg)

因为每种方案都其适用场景和缺点，因此在实际应用中，会根据实际情况选择方案。例如：如果大量对象都是长期「存活」，则分代处理优势也不大。

> 原文链接：Know Your Engines: How to Make Your JavaScript Fast  
> [http://t.cn/RIROY1W](http://conferences.oreilly.com/velocity/velocity2011/public/schedule/detail/18087)

### 查看 Chrome 浏览器下的 CG 过程

1. 使用快捷键 `F12` 或者 `Ctrl+Shift+J` 打开 Chrome 浏览器的「开发者工具」。
2. 选择 `Timeline` 选项卡，在 `Capture` 选项中，只勾选 `Memory`。
3. 设置完成后，点击最左边的 `Record` 按钮，然后就可以访问网页了。
4. 打开一个网站，例如：[http://www.taobao.com](http://www.taobao.com)，当网页加载完成后，点击 `Stop`，等待分析结果。
5. 然后在 `Chart View` 上寻找内存急速下降的部分，查看对应的 `Event Log`，可以从中找到 GC 的日志。

具体过程如下图所示：
![](http://qiniu.shijiajie.com/blog/javascript-lesson/2.6-GC&MemoryManagement/WechatIMG127.jpg)

## 关卡

- 挑战一，尝试写一段小程序，触发 IE6 的无限 CG。
- 挑战二，参考「查看 Chrome 浏览器下的 CG 过程」，尝试查看 Firefox  浏览器下的 CG 过程。

## 更多

> 关注微信公众号「劼哥舍」回复「答案」，获取关卡详解。  
> 关注 [https://github.com/stone0090/javascript-lessons](https://github.com/stone0090/javascript-lessons)，获取最新动态。