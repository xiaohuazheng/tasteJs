## JavaScript-判断


代码中，多多少少会有判断语句。业务越复杂，逻辑就越复杂，判断就越多

### 比较判断

比较判断是比较两个值，返回一个布尔值，表示是否满足比较条件。JavaScript一共提供了8个比较运算符，参考我另一篇文章：[JavaScript-运算符浅析][1]。

这里主要说一下严格相等运算符和相等运算符的区别：

`==`相等运算符比较两个值的时候会判断两个值的类型，如果不是同一类型，会强制转换为同一类型进行比较（类型转换参考温故js系列第一篇文章： [类型转换][2]）。

而`===`比较两个值的时候先判断两个值的类型，如果不是同一类型，直接返回false，值类型相同再进行值的比较。

所以，从性能上来说，`==`会比`===`多走一条类型转换的路，稍逊一点。从结果上来说，有时候类型转换会给你带来你不想要的比较结果。 这也就是为什么都推崇使用`===`的原因。当然，`==`在合适的业务场景下使用也是必要的。

#### 严格相等运算符=== 

            判断                        返回
    两个值类型不同                       false
    两个值都是null/undefined/true/false  true      
    两个值其中之一为NaN                  false
    两个值都为数值且值相等                true
    两个值都为字符串且值相等              true
    两个值都指向同一个引用类型            true
    
    1 === "1" // false
    true === true // true
    undefined === undefined // true
    null === null // true
    1 === 1 // true
    NaN === NaN  // false
    +0 === -0 // true
    ({} === {}) // false
    [] === [] // false
    (function (){} === function (){}) // false
    var v1 = {};
    var v2 = v1;  //两个值引用同一个对象
    v1 === v2 // true

严格相等运算符有一个对应的严格不相等运算符（!==），两者的运算结果正好相反

#### 相等运算符== 

    规则： 
    
    if 相等运算符比较相同类型的数据时，同严格相等运算符
    else if 相等运算符比较不同类型的数据时：
    原始类型的数据会转换成数值类型，把字符串和布尔值都转为数值，再进行比较
    null == undefined  返回true
    一个是对象，另一个是数字或者字符串，把对象转成基本类型值再比较
    else false
    
    123 == 123; //true
    '123' == 123; //true，'123'会转成成数值123
    false == 0; //true，false 转成数值就是0
    'a' == 'A'; //false，转换后的编码不一样
    123 == {}; //false，执行toString()或valueOf()会改变
    123 == NaN; //false，只要有NaN，都是false
    {} == {}; //false，比较的是他们的地址，每个新创建对象的引用地址都不同
    
    null == undefined //true
    'NaN' == NaN //false
    123 == NaN //false
    NaN == NaN //false
    false == 0 //true
    true == 1 //true
    true == 2 //false
    undefined == 0 //false
    null == 0 //false
    '123' == 123 //true
    '123' === 123 //false

相等运算符有一个对应的不相等运算符（!=），两者的运算结果正好相反

### !!判断

!!相当于Boolean，写代码时用!!转化为Boolean类型做判断非常好用

    !!'xzavier';   // true
    !!'';          // false
    !!'0';         // true
    !!'1';         // true
    !!'-1'         // true
    !!0            // false
    !!undefined    // false
    !!null         // false
    !!NaN          // false
    !!{};          // true
    !!{name:'xz'}  // true
    !![];          // true
    !![1,2,3];     // true
    !!true;        // true

### !判断

取反运算符 ! 用于将布尔值变为相反值，即true变成false，false变成true。对于非布尔值的数据，取反运算符会自动将其转为布尔值。规则是，以下六个值取反后为true，其他值取反后都为false

    undefined  null  false  0(包括+0和-0)  NaN  空字符串('')
    
    !undefined    // true
    !null         // true
    !false        // true
    !0            // true
    !NaN          // true
    !""           // true    
    !54           // false
    !'hello'      // false
    ![]           // false
    ![1,2,3]      // false
    !{}           // false
    !{name:'xz'}  // false

### []和{}判断

对于[]和{}，我们在业务代码中肯定会对其做判断，如上
    !!{};          // true
    !!{name:'xz'}  // true
    !![];          // true
    !![1,2,3];     // true

不能用!!和!做判断，对于数组，我们用它的length属性做判断

    [].length       // 0 false
    [1,2,3].length  // 3 true

对象的话，可以采用jQuery的方法$.isEmptyObject(obj)，也可以js封装一个方法，就模仿jQuery 写一个

    function isEmptyObject(obj) {
    	var name;
    	for ( name in obj ) {
    		return false;
    	}
    	return true;
    }
    isEmptyObject({});  //true
    isEmptyObject({name: 'xzavier'});  false 

推荐一个工具库underscore，它也有个方法isEmpty(object)

    const _ = require('underscore');
    _.isEmpty({});  // true

### &&判断

用在条件表达式中，规则是：

    num1 && num2
    true    true    true
    true    false   false
    false   true    false
    false   false   false

用在语句中，规则是 ： 

    result = expression1 && expression2

如果expression1的计算结果为false，则result为expression1。否则result为expression2

    (1 - 1) && ( x += 1)  // 0
    (2 > 1) && ( 5 + 5)   // 10
    (2 + 1) && ( 5 + 5)   // 10

### ||判断

用在条件表达式中，规则是：

    num1 || num2
    true    true     true
    true    false    true
    false   true     true
    false   false    false

用在语句中，规则是：

    如果第一个运算子的布尔值为true，则返回第一个运算子的值，且不再对第二个运算子求值
    如果第一个运算子的布尔值为false，则返回第二个运算子的值

||运算符一般在业务代码中做条件表达式判断和容错处理，我们在取数据时取不到的情况下，又不能影响后面的业务代码，就需要进行容错。||就是一个非常好的容错写法，相当于提供一个备用数据。

    var data = undefined || backup_data;  //请求出错，数据为undefined时，就去备用数据backup_data

### 三目判断

规则：

    condition ? expression1 : expression2;
    
    function absN(xzavier) {
        return xzavier > 0 ? xzavier : -xzavier;
    }
    absN(-123);  //123
    absN(123);  //123

如果第一个表达式的布尔值为true，则返回第二个表达式的值，否则返回第三个表达式的值。

判断暂时写到这儿，判断是我们代码生涯中时时刻刻接触的，更多的attention在接触研究过会更新于此...

休息一刻，约好要去打篮球了。。。


  [1]: https://segmentfault.com/a/1190000005927342#articleHeader3
  [2]: https://segmentfault.com/a/1190000005863067
