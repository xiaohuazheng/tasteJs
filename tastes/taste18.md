前端学习：[教程&模块化/规范化/工程化/优化&工具/调试&值得关注的博客/Git&面试资源汇总][1]

JavaScript一路走来，备受争议，与其说它备受争议，不如说它不够完美。不够完美？那完美了还得了，它的强大你还没体会到吗？它是如此的灵活，当然随之而来的便是开发的代价，它不像强类型语言那样规规矩矩，今天就说说这个加法运算符。当然，这个不是之前的温故，不是我说，随意翻译，顺便分享，后附原文。

这里不讲+转换类型，详见第四章运算符详解

### 本职工作：加法运算符

    var result = 10 + 5;  
    // number + number = number (addition)
    // 15
    

关于运算符的学习可参考：[运算符详解][3]

### 胜任工作：连接字符串

    var result = "Hello, " + "World!";  
    // string + string = string (concatenation)
    // "Hello, World!"

JavaScript允许我们在`object、array、null or undefined`上使用操作符。那我们来看看使用的规则和细节。

### 转换规则

    operand + operand = result 

 - 如果操作数中有一个是对象，它会被转换为原始值`(string、number or boolean)`
 - 如果操作数中有一个是字符串，第二个操作数将转换成字符串，并且连接在一起转换为字符串
 - 在其它情况之下，两个操作数转换为数字并执行加法运算

如果两个运算数是原始类型，则检查如果至少一个操作数是字符串的话，就把它们当字符串连接。在其他情况下，它会把他们都当数字，然后转化为数字相加的总和。

### 对象转换规则

 - 如果对象类型是一个`Date`，使用`toString()`方法
 - 在其它情况下使用`valueOf()`方法，返回一个原始值
 - 如果`valueOf()`方法不能返回一个原始值，使用`toString()`方法。大多情况都会发生这种情况

当一个数组被转换为原始值，JavaScript会使用`join(',')`方法，例如`[1,5,6]`的原始值是`"1,5,6"`。一个普通的JavaScript对象`{}`的原始值是`"[object Object]"`

## Learning from examples

阅读实例请参看上面的转换规则

### Example 1: 数字和字符串

    var result = 1 + "5"; // "15"

解释：

 - `1 + "5"` （第二个操作数是一个字符串，那么数字1将会变成字符串"1"）
 - `"1" + "5"` （连接字符串）
 - `"15"`

第二个操作数是一个字符串,第一个操作数把`number`转换成`string`类型，然后将它们连接在一起。

### Example 2:数字和数组

    var result = [1, 3, 5] + 1; //"1,3,51"

解释：

 - `[1,3,5] + 1` （数组[1,3,5]转换为原始值为"1,3,5"）
 - `"1,3,5" + 1` （数字1转换成字符串"1"）
 - `"1,3,5" + "1"` （连接字符串）
 - `"1,3,51"`

第一个操作数是数组，转换为原始值字符串，第二个操作数是数字，转换为字符串，然后两个字符串连接在一起。

### Example 3:数字和布尔值

    var result = 10 + true; //11 

解释：

 - `10 + true` （布尔值true将转换为数字1）
 - `10 + 1` （数字做加法运算）
 - `11`

因为两个操作数都不是字符串，布尔值将转换为数字符，然后作数字加法运算。

### Example 4: 数字和对象

    var result = 15 + {}; // "15[object Object]"

解释：

 - `15 + {}` （第二个操作数是一个对象，对象转换为字符串"[object Object]"）
 - `15 + "[object Object]"` （数字15转换为字符串"15"）
 - `"15" + "[object Object]"` （连接字符串）
 - `"15[object Object]"`

第二个操作数是一个对象，转换为原始值字符串。因为`valueOf()`方法返回的是对象本身，而不是一个原始值，所以再使用`toString()`方法，返回一个字符串。

第二个操作数转换之后是一个字符串，故数字也将转换为一个字符串，再把字符串连接在一起。

### Example 5:数字和`null`

    var result = 8 + null; // 8

解释：

 - `8 + null`（因为操作数没有字符串，null将转换为数字0）
 - `8 + 0` （两个数字做加法运算）
 - `8`

如果操作数不是对象或字符串时，`null`会转换为数字，然后做数字加法运算。

### Example 6:字符串和`null`

    var result = "queen" + null; // "queennull"

解释：

 - `"queen" + null` （因为第一个操作数是一个字符串，null将转换为一个字符串"null"）
 - `"queen" + "null`" （连接字符串）
 - `"queennull"`

因为第一个操作数是一个字符串，所以`null`将转换为一个字符串`"null"`，然后两个把字符串连接在一起。

### Example 7: 数字和`undefined`

    var result = 12 + undefined; // NaN

解释：

 - `12 + undefined` （因为没有任何一个操作数是字符串，undefined将转换为一个数字NaN）
 - `12 + NaN` （做数字加法运算）
 - `NaN`

因为没有操作数是对象或字符串，`undefined`将转换为`NaN`。两个数字做加法运算，之所以要做加法，是因为`typeof(NaN) == "number"`，又因为任何一个数字和`NaN`做加法运算，所以等于NaN。

### 结论

以避免潜在的问题，不使用加法运算符处理对象，除非你清楚地使用`toString()`或`valueOf()`方法。

如在实例中看到的，开发中要明确场景的转换规则，以防JavaScript给你带来的惊喜哦。

Have a good coding day!


[See the examples in JS Bin][4]

英文：[JavaScript addition operator in details][5]


### MORE延伸几个表达式

    [] + []; //''
    [] + {}; //'[object Object]'
    {} + {}; //NaN  firfox下结果
    {} + {}; //'[object Object][object Object]' chrome下结果
    ({} + {}); //'[object Object][object Object]'
    {} + []; //0


  [1]: https://github.com/xiaohuazheng/-/issues/1
  [3]: https://segmentfault.com/a/1190000005927342
  [4]: http://jsbin.com/fiwemir/2/edit?js,console
  [5]: https://rainsoft.io/javascriptss-addition-operator-demystified/