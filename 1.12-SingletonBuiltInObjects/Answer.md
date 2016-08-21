# 关卡详解

```
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

第2步，开始取 i-99 范内的随机数，把每次取到的随机数作为位置（**数组的下标**）与位置（**数组的下标**）为 i 的数交换数值。这样做的意义是，将已经取到的随机数在取值范围中排除，下一次去随机数仅会在剩下的数字中取。

第2步不太容易理解，举个栗子：假设第一次取到的随机数是39，把 **位置39的数** 与 **位置0的数** 交换之后，再从 **位置1** 开始看该数组，你会惊奇的发现，剩下的是0-99除39以外的所有数字，但它们的位置是1-99，接下来我们仅需要从1-99中取一个随机数，作为数组下标，即可在剩下的数字中取随机数了，以此类推。