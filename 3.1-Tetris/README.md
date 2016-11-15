# 过五关斩六将 - 带你用 JavaScript 开发「俄罗斯方块」小游戏

历时近5个月，终于完成了[《JavaScript 闯关记》](https://github.com/stone0090/javascript-lessons)的理论基础部分18篇文章的整理，没想到能在 [Github](https://github.com) 上获得600的 Star，真是非常开心，你们的支持就是我写作的动力，谢谢大家。

接下来我们即将进入「紧张、刺激、好玩」的实践部分，我会引导大家结合 JavaScript 理论基础来完成一些有趣的功能和效果。

此次我们的主题是，**带你用 JavaScript 开发「俄罗斯方块」小游戏**。这是我在2012年刚转 Web 开发时，写过的第一段 JavaScript 代码。当时也是参考别人的博客照着写的，写的时候发现自己明明「看懂了，却写不出来」，多次回顾的源码之后才勉强完成。原文地址：[`http://t.cn/zORubdi`](http://blog.csdn.net/shi0090/article/details/7390105)，演示地址：[`http://t.cn/RfUusy0`](http://shijiajie.com/other/javascript-game/tetris)。

所以实践部分，大家一定要跟着我写代码，只有「看懂了，并且写得出来」，这个知识你才真正掌握了。

> 此次的实践，大家最好能具备「熟悉 HTML / CSS / JavaScript」的能力，如果火候稍有欠缺也没关系，在学习过程中随时在 [菜鸟教程`http://www.runoob.com`](http://www.runoob.com/) 查阅相关资料也能跟上，只是进度会稍慢一些。

## 正式开始

从现在开始，我会给出一系列的关卡挑战，引导大家逐步实现「俄罗斯方块」小游戏。如果你能完成全部挑战，说明你本来就具备这个能力，只是未曾试过而已。

### 关卡一：操作单个 `div` 移动。

- 新建一个 html 页面，在该页面上添加一个 `div`。
- 给该 `div` 添加样式 `.activityModel { margin: 1px; width: 19px; height: 19px; background: red; position: absolute; }`。
- 使用键盘的「上下左右」键可操作 `div` 移动，每次移动 `20px`。

> 尽量自己完成，不参考我的代码。  
> 源码地址：[`http://t.cn/RfGmjjz`](https://github.com/stone0090/javascript-lessons/blob/master/3.1-Tetris/js-tetris-001.html)。  
> 演示地址：[`http://t.cn/Rfbuawp`](http://sandbox.runjs.cn/show/bfc1xql3)。  
> ![](http://qiniu.shijiajie.com/blog/javascript-lesson/3.1/js-tetris-001.gif)  

### 关卡二：操作多个 `div` 整体移动。

- 在该页面上再加入三个 `div`，组合成下图中的方块。
- 使用键盘的「上下左右」键可操作方块移动，每次移动 `20px`。
- 使用键盘的「空格」键可操作方块旋转。

> 尽量自己完成，不参考我的代码。  
> 源码地址：[`http://t.cn/RfGmTyR`](https://github.com/stone0090/javascript-lessons/blob/master/3.1-Tetris/js-tetris-002.html)。  
> 演示地址：[`http://t.cn/RfG2hd1`](http://sandbox.runjs.cn/show/htxm5rno)。  
> ![](http://qiniu.shijiajie.com/blog/javascript-lesson/3.1/js-tetris-002.gif)  
> 提示：首先，把每个 `div` 当看成是坐标轴里的点，使用坐标来动态绘制方块会更简单。然后，找出方块旋转时坐标改变的规律，利用这个规律操作方块旋转。本关卡的方块可用坐标`2,0``2,1``2,2``1,2`表示，其余六个方块可分别用坐标：`0,1``1,1``2,1``3,1 `、`1,0``1,1,``1,2``2,2`、`0,1``1,1``1,2``2,2`、`1,2``2,2``2,1``3,1`、`1,1``2,1``1,2``2,2`、`0,2``1,2``1,1``2,2`表示。  

### 关卡三：操作多个 `div` 在某个范围内移动和旋转。

- 在该页面上再添加一个 `div`，给该 `div` 添加样式 `.container { top: 0px; left: 0px; width: 200px; height: 360px; background: black; position: absolute; }`，该 `div` 用来表示一个18行10列的容器。
- 限制方块只能在该容器内部移动和旋转。

> 尽量自己完成，不参考我的代码。  
> 源码地址：[`http://t.cn/RfGmHuv`](https://github.com/stone0090/javascript-lessons/blob/master/3.1-Tetris/js-tetris-003.html)。  
> 演示地址：[`http://t.cn/RfGnp98`](http://sandbox.runjs.cn/show/qgg31pmy)。  
> ![](http://qiniu.shijiajie.com/blog/javascript-lesson/3.1/js-tetris-003.gif)  

### 关卡四：操作多个 `div` 自动下落，若碰到容器底部，便停下来并变成灰色。

- 操作方块以600毫秒的速度，匀速从容器顶部开始下落，每次下落 `20px`。
- 保持「左右下」键的操作不变，修改「上」键的操作为方块旋转，修改「空格」键的操作为方块急速下落。
- 若方块的底部，触碰到容器的底部，便停下来并变成灰色。
- 然后再创建一个新的方块继续下落，周而复始。

> 尽量自己完成，不参考我的代码。  
> 源码地址：[`http://t.cn/RfqOix9`](https://github.com/stone0090/javascript-lessons/blob/master/3.1-Tetris/js-tetris-004.html)。  
> 演示地址：[`http://t.cn/Rf5cf9I`](http://sandbox.runjs.cn/show/duw0x6t2)。  
> ![](http://qiniu.shijiajie.com/blog/javascript-lesson/3.1/js-tetris-004.gif)  

### 关卡五：操作多个 `div` 之间不会互重叠，若铺满一行，便自动消除。

- 从七个基本方块中，随机取一个，以600毫秒的速度，匀速从容器顶部开始下落，每次下落 `20px`。
- 若方块的底部，触碰到容器的底部，或触碰到其他方块，便停下来并变成灰色。
- 若下落的方块正好铺满一行，便自动消除该行。
- 然后再从七个基本方块中，随机取一个继续下落，周而复始。

> 尽量自己完成，不参考我的代码。  
> 源码地址：[`http://t.cn/RfqOKkc`](https://github.com/stone0090/javascript-lessons/blob/master/3.1-Tetris/js-tetris-005.html)。  
> 演示地址：[`http://t.cn/Rf5couT`](http://sandbox.runjs.cn/show/jebpssof)。  
> ![](http://qiniu.shijiajie.com/blog/javascript-lesson/3.1/js-tetris-005.gif)  

### 关卡六：对「俄罗斯方块」小游戏的功能进行完善。

- 增加方块超过容器顶部，游戏自动结束功能。
- 增加按「回车」键暂停功能。
- 增加预告功能。
- 增加计分功能。
- 等等。

> 本关卡没有示例代码，请大家自行发挥。

## 总结和展望

至此，「俄罗斯方块」小游戏就开发完了，它涉及了[《JavaScript 闯关记》](https://github.com/stone0090/javascript-lessons)中绝大部分知识点，能够独立完成以上六关，说明你对 JavaScript 已经了解颇深，可以试着参与实际项目了。

说实话，这个项目中的示例代码写的很一般，基本没有封装可言，但这就是我在2012年初学 JavaScript 的水平，之所以把当时的代码原汁原味的展现出来，是为了让大家体会一个 JavaScript 初学者的编码思路。

下一步，我会用面向对象的编码风格，以及前端工程化的方式去完善它。所以我在 GitHub 上新建了 [`https://github.com/stone0090/javascript-tetris`](https://github.com/stone0090/javascript-tetris) 项目，准备持续不断的为其加入新功能。如果大家对此也有兴趣，或者有什么新奇的想法，欢迎在 [Issues](https://github.com/stone0090/javascript-tetris/issues) 提交给我，更欢迎大家直接解决 [Issues](https://github.com/stone0090/javascript-tetris/issues) 中的问题提交代码给我。项目截图如下：
 ![](http://qiniu.shijiajie.com/blog/javascript-lesson/3.1/js-tetris-007.jpg)

## 更多

> 关注微信公众号「劼哥舍」，老斯基带你飞。
> 关注 [https://github.com/stone0090/javascript-lessons](https://github.com/stone0090/javascript-lessons)，获取最新动态。
