# 手把手教你 - 用 JavaScript 开发「俄罗斯方块」小游戏
历时近5个月，终于完成了[《JavaScript 闯关记》](https://github.com/stone0090/javascript-lessons)的理论基础部分18篇文章的整理，能在 [Github](https://github.com) 上获得600的 Star 真是非常开心，你们的支持就是我写作的动力，谢谢大家。

接下来我们即将进入「紧张、刺激、好玩」的实践部分，我会抛出一个又一个的实际问题，引导大家结合 JavaScript 理论基础来完成一些有趣的功能和效果。此次的实践，大家最好能具备「熟悉 HTML / CSS / JavaScript」的能力，如果火候稍有欠缺也没关系，在学习过程中随时在 [菜鸟教程`http://www.runoob.com`](http://www.runoob.com/) 查阅相关资料也能跟上，只是进度会稍慢一些。

这次我们的主题是，**用 JavaScript 开发「俄罗斯方块」小游戏**。这是我在2012年刚转 Web 开发时做的第一练手项目，当时在 CSDN 看到相关代码便想仿写一个。自己写的时候才发现，明明「看懂了，却写不出来」，多次回顾的源码之后才勉强完成。原文地址：[`http://t.cn/zORubdi`](http://blog.csdn.net/shi0090/article/details/7390105)，演示地址：[`http://t.cn/RfUusy0`](http://shijiajie.com/other/javascript-game/tetris)。

所以实践部分，大家一定要跟着我写代码，只有「看懂了，并且写得出来」，这个知识你才真正掌握了。

## 正式开始

从现在开始，我会给出一系列的关卡挑战，引导大家逐步实现「俄罗斯方块」小游戏。如果你每关的代码都和我的相似，说明你完全有能力自己独立实现「俄罗斯方块」小游戏，只是未曾试过而已。若是觉得我的示例代码很烂（初学 JavaScript 所写的代码，难免惨不忍睹嘛），欢迎你在 [`https://github.com/stone0090/javascript-tetris`](https://github.com/stone0090/javascript-tetris) 上提交更优质代码给我，让我们一起来完善这个「俄罗斯方块」小游戏，有了大家的参与，说不定它会成为像「2048」或「你画我猜」一样热门小游戏呢！

### 关卡一
1.新建一个 html 页面，在该页面上添加一个 `div`。
2.给该 `div` 添加样式 `.activityModel { margin: 1px; width: 19px; height: 19px; background: red; position: absolute; }`。
3.使用键盘的「上下左右」可操作 `div` 移动，每次移动 `20px`。
> 大家完成之后，可参考下面示例代码。
> 源码地址：[`http://t.cn/RfbuX9X`](http://runjs.cn/code/bfc1xql3)。
> 演示地址：[`http://t.cn/Rfbuawp`](http://sandbox.runjs.cn/show/bfc1xql3)。
> ![](http://qiniu.shijiajie.com/blog/javascript-lesson/3.1/js-tetris-001.gif)

### 关卡二
1.新建一个 html 页面，在该页面上添加四个 `div`，组合成下图中的方块。
2.使用键盘的「上下左右键」可操作方块移动，每次移动 `20px`。
3.使用键盘的「空格键」可操作方块旋转。
> 大家完成之后，可参考下面示例代码。
> 源码地址：[`http://t.cn/RfG2zBK`](http://runjs.cn/code/htxm5rno)。
> 演示地址：[`http://t.cn/RfG2hd1`](http://sandbox.runjs.cn/show/htxm5rno)。
> ![](http://qiniu.shijiajie.com/blog/javascript-lesson/3.1/js-tetris-002.gif)
> 提示：把每个 div 当成坐标轴里的点，使用坐标来动态绘制方块（上图中的方块，可用坐标`2,0`，`2,1`，`2,2`，`1,2`表示），可从方块旋转时坐标改变的规律得到一个公式，利用公式改变坐标，从而控制方块旋转。然后按照个思路可以很容易的绘制其他形状的方块。

### 关卡三
1.新建一个 html 页面，在该页面上添加一个 `div`。
2.给该 `div` 添加样式 `.container { top: 0px; left: 0px; width: 200px; height: 360px; background: black; position: absolute; }`，该 `div` 用来表示俄罗斯方块18行10列的容器。
3.把关卡二写好的方块放到该容器 `div` 中，使其只能在容器内部移动和旋转。
> 大家完成之后，可参考下面示例代码。
> 源码地址：[`http://t.cn/RfbuX9X`](http://runjs.cn/code/bfc1xql3)。
> 演示地址：[`http://t.cn/RfGnp98`](http://sandbox.runjs.cn/show/qgg31pmy)。
> ![](http://qiniu.shijiajie.com/blog/javascript-lesson/3.1/js-tetris-003.gif)

### 第四步
### 第五步
### 第六步
### 第七步
### 第八步
### 第九步
### 

### 程序分析
因为以前写过，所以在数据结构上还是有点映像，游戏区就对应着一个二维数组。每个图形就是一组有着相对位置关系的坐标，当然还有颜色定义。

所有行为都是通过数据（坐标）的变化来实现的。而障碍物（已固定下来的小方块）判断则是通过当前图形位置及定义中所有小方块的相对位置计算出各小方块坐标之后检查大矩阵对应坐标是否存在小方块数据来判断。这需要提前计算出当前图形在下一个形态所需要占用的坐标列表。

方块的自动下落是通过时钟周期控制。如果还要处理消除动画，就可能需要两个时钟周期控制。当然可以取两个时钟周期的了大公约数来合并成一个公共时钟周期，但俄罗斯方块的动画相当简单，似乎没有必要进行这么复杂的处理——可以考虑在消除时暂停下落时钟周期，消除完成之后再重启。

交互部分主要靠键盘处理，只需要给 document 绑定 keydown 事件处理就好。

## 知识点总结

## 下一步展望






- 拆分代码，一步步引导大家完成JavaScript俄罗斯方块的代码
  - 建立方块模型
  - 建立方块类，及控制
  - 建立容器类，及边界控制
- 总结