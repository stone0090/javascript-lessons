# 关卡详解

```javascript
// 如何高效产生m个n范围内的不重复随机数（m<n)
var getRandomNumber = function(n, m){

    if(typeof n !== 'number' || typeof m !== 'number'){
        throw Error('m和n必须是数字！');
    }

    if(m >= n){
        throw Error('m必须小于n！');
    }

    var arr = [],
        result = [],
        i = 0,
        j = 0,
        tempNumber = 0,
        randomNumber = 0;

    function selectFrom(lowerValue, upperValue) {
        var choices = upperValue - lowerValue + 1;
        return Math.floor(Math.random() * choices + lowerValue);
    }

    for(; i < n; i++){
        arr[i] = i;
    }

    for(; j < m; j++){
        randomNumber = selectFrom(j, n - 1);
        tempNumber = arr[j];
        arr[j] = arr[randomNumber];
        arr[randomNumber] = tempNumber;
        result.push(arr[j]);
    }

    return result;
}

console.log(getRandomNumber(100, 50));
```

假设取50个100范围内的不重复随机数，思路分析：

第1步，为数组的每个数字按其位置（**数组的下标**）赋值，我们获得一个 **100个数字**、**顺序排列** 的数组。

第2步，开始取 i-99 范内的随机数，把每次取到的随机数作为位置（**数组的下标**）与位置（**数组的下标**）为 i 的数交换数值。这样做的意义是，将已经取到的随机数在取值范围中排除，下一次仅会在剩下的数字中取随机数。

第2步不太容易理解，举个栗子：假设第一次取到的随机数是39，把 **位置39的数** 与 **位置0的数** 交换之后，再从 **位置1** 开始看该数组，你会惊奇的发现，剩下的是0-99除39以外的所有数字，但它们的位置是1-99，接下来我们仅需要从1-99中取一个随机数，作为数组下标，即可在剩下的数字中取随机数了，以此类推。



## 答疑1

部分朋友对下面这个函数有疑问，认为其取值范围是 `lowerValue` 到 `upperValue + 1` ，但这是不对的。

```javascript
// 取 lowerValue 到 upperValue 之间的随机数（可包含 lowerValue 和 upperValue 本身）。
function selectFrom(lowerValue, upperValue) {
  var choices = upperValue - lowerValue + 1;
  return Math.floor(Math.random() * choices + lowerValue);
}
```

首先，我们看看该函数用到的两个 `Math` 函数是如何定义的：

- `Math.random()` 方法返回介于0和1之间一个随机数，不包括0和1。
- `Math.floor()` 执行向下舍入，即它总是将数值向下舍入为最接近的整数。

然后，模拟该函数的运算过程。

- 假设我们使用 `selectFrom(0, 49)` 调用该函数。
- 计算 `upperValue - lowerValue + 1` ，得出局部变量 `choices` 的值为 `50`。
- 计算 `Math.random() * choices + lowerValue` ，可得到一个 `0` 到 `50` 之间的随机数，这个数可能是：`0.28113140100372025`，`38.86468755785983`，`49.03196313214102`，但不会是 `0` 和 `50` 本身。
- 计算 `Math.floor(0 到 50 之间的随机数)` ，可得到一个 `0` 到 `49` 的整数。



## 答疑2

一位叫 Joker 的朋友发来自己仿写的方法，以下是我给出评析。

```javascript
/**
 * Created by Joker on 2016/10/9.
 * github:justjoker
 * email:573186230@qq.com
 */
var getRandomNumberByCount = function (n, m) {
    (typeof n !== 'number' || typeof m !== 'number') && console.log('m和n必须是数字!');
    !(m >= n) || console.log('m必须小于n!');
    var result = [];

    function selectRandom(lowerValue, upperValue) {
        var choice = upperValue - lowerValue + 1;
        return Math.floor(Math.random() * choice + lowerValue);
    }

    function pushRandom() {
        var random = selectRandom(0, n - 1);
        result.indexOf(random) < 0 && result.push(random);
    }

    // var i = 0;
    while (result.length < m) {
        pushRandom();
        /* i++;
         console.log(i);
         console.log("结果长度约:" + result.length);*/
        !(result.length == m) || (console.log(result.sort(function (current, next) {
            return current - next;
        })));
    }
}
getRandomNumberByCount(100, 50);
```

> 首先，值得肯定的是，你的解题思路正确，方法可以得出正确结果。
>
> 但这种解法不是最优的，你每次都在 `n` 个数中取随机数，有几率会取到重复的数字，`m` 和 `n` 越接近，取到重复数字的概率就越大。
>
> 而我的解题思路是，将已经取到的随机数在取值范围中排除，下一次仅会在剩下的数字中取随机数，所以不存在重复取值的情况。具体分析过程请查看原文。
>
> 最后再给你一点小建议，虽然你的代码风格特别简洁明了，但对 JavaScript 新手不太友好，建议简洁和易读性同事兼顾，这样就能让更多的人喜欢读你的代码。