# 关卡详解

按要求完成下列常用的正则表达式。

``` javascript
// 挑战一：数字
var pattern1 = /^[0-9]*$/;
console.log(pattern1.test('123')); // true
console.log(pattern1.test('abc')); // false
```

``` javascript
// 挑战二：3位的数字
var pattern2 = /^\d{3}$/;
console.log(pattern2.test('123'));  // true
console.log(pattern2.test('1234')); // false
```

``` javascript
// 挑战三：至少3位的数字
var pattern3 = /^\d{3,}$/;
console.log(pattern3.test('1234')); // true
console.log(pattern3.test('12'));   // false
```

``` javascript
// 挑战四：3-5位的数字
var pattern4 = /^\d{3,5}$/;
console.log(pattern4.test('1234')); // true
console.log(pattern4.test('1'));    // false
```

``` javascript
// 挑战五：由26个英文字母组成的字符串
var pattern5 = /^[A-Za-z]+$/;
console.log(pattern5.test('abc'));  // true
console.log(pattern5.test('1abc')); // false
```

``` javascript
// 挑战六：由数字和26个英文字母组成的字符串
var pattern6 = /^[A-Za-z0-9]+$/;
console.log(pattern6.test('1abc'));  // true
console.log(pattern6.test('_abc'));  // false
```

``` javascript
// 挑战七：日期格式：年-月-日
var pattern7 = /^(\d{4}|\d{2})-((1[0-2])|(0?[1-9]))-(([12][0-9])|(3[01])|(0?[1-9]))$/;
console.log(pattern7.test('2016-08-20'));  // true
console.log(pattern7.test('2016/08/20'));  // false
```

``` javascript
// 挑战八：时间格式：小时:分钟, 24小时制
var pattern8 = /^((1|0?)[0-9]|2[0-3]):([0-5][0-9])$/;
console.log(pattern8.test('13:45'));  // true
console.log(pattern8.test('13点45')); // false
```

``` javascript
// 挑战九：中国大陆身份证号，15位或18位
var pattern9 = /^\d{15}(\d\d[0-9xX])?$/;
console.log(pattern9.test('422322199901090033'));  // true
console.log(pattern9.test('asdfadfasfasdf1234'));  // false
```