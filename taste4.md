## JavaScript-运算符

JavaScript 有一系列操作数据值的运算符，运算符按照特定运算规则对操作数进行运算，将简单的表达式组合成复杂的表达式。

### 一元运算符

一元运算符只能操作一个值。

累加累减运算符：
 
    var xzavier = 123;
    xzavier++  //把变量累加1，相当于xavier = xavier + 1
    ++xzavier  //把变量累加1，相当于xavier = xavier + 1
    xzavier--  //把变量累减1，相当于xavier = xavier - 1
    --xzavier  //把变量累减1，相当于xavier = xavier - 1
    

上述代码不只是`++--`前后置的区别，当有赋值操作时，区别为：

    var xzavier = 1;
    var num1 = ++xzavier; //num1 值为2
    var num2 = xzavier++; //num2 值为1

加减运算符本应参与运算，但也可以进行类型转换：

    var xzavier1 = 'xzavier', xzavier2 = '123', xzavier3 = false, xzavier4 = 123, xzavier5 = '-123';
    +xzavier1  //NaN
    +xzavier2  //123
    +xzavier3  //0
    +xzavier4  //123
    +xzavier5  //-123
    -xzavier1  //NaN
    -xzavier2  //-123
    -xzavier3  //0
    -xzavier4  //-123
    -xzavier5  //123

当然，还有一些方法也可以被当做一元运算符，比如：

- typeof 方法是一元运算符，可操作单个值，判断类型。
- delete 也是一元运算符， 它用来删除对象属性或者数组元素。


### 算术运算符

在运算时候如果运算值不是数值，那么后台会先使用 `Number()` 转型函数将其转换为数值，隐式转换：

加法

    var xzavier = 123 + 456;     //579
    var xzavier = 1 + NaN;       //NaN，只要运算中有一个NaN，计算值就为NaN
    var xzavier = 123 + 'abc';   //123abc  有字符串时未字符串连接符
    var xzavier = 123 + Object;  //123[object Object]

对象会内部调用 `toString()` 或 `valueOf()` 方法进行转换为原始值。（这里有提到 valueOf 和 toString 方法的转换：[JavaScript-数据类型浅析][1]）

减法

    var xzavier = 123 - 12; //111
    var xzavier = -123 - 12 //-135
    var xzavier = 123 - true; //122 true会隐式转换为1
    var xzavier = 123 - 'xzavier'; //NaN

乘法

    var xzavier = 123 * 2; //246
    var xzavier = 123 * NaN; //NaN
    var xzavier = 123 * true; //123
    var xzavier = 123 * ''; //0

除法

    var xzavier = 123 / 3; //41
    var xzavier = 123 / 4; //30.75
    var xzavier = 123 / NaN; //NaN
    var xzavier = 123 / true; //123
    var xzavier = 123 / ''; //Infinity

求余

    var xzavier = 123 % 3; //0
    var xzavier = 123 % 4; //3
    var xzavier = 123 % NaN; //NaN
    var xzavier = 123 % true; //0


### 关系运算符

用于比较的运算符称作为关系运算符：小于 `<`、大于 `>`、小于等于 `<=`、大于等于 `>=`、相等 `==`、不等 `!=`、全等(恒等) `===`、不全等(不恒等) `!==`：

1. 两个操作数都是数值，则数值比较；

2. 两个操作数都是字符串，则比较两个字符串对应的字符编码值；

3. 两个操作数有一个是数值，则将另一个转换为数值，再进行数值比较；

4. 两个操作数有一个是对象，则先调用 `valueOf()` 方法或 `toString()` 方法，再用结果比较。


    321 > 123; //true

    123 > 321; //false

    '123' > 321; //false

    '321' > '1234'; //true

    'a' > 'b'; //false a=97,b=98

    'a' > 'B'; //true B=66

    1 > Object; //false

在相等和不等的比较上，如果操作数是非数值，则遵循一下规则：

1. 一个操作数是布尔值，则比较之前将其转换为数值，false 转成 0，true 转成 1；

2. 一个操作数是字符串，则比较之前将其转成为数值再比较；

3. 一个操作数是对象，则先调用 `valueOf()` 或 `toString()` 方法后再和返回值比较；

4. 不需要任何转换的情况下，null 和 undefined 是相等的；

5. 一个操作数是 NaN，则 `==` 返回 false，`!=` 返回 true；并且 NaN 和自身不等；

6. 两个操作数都是对象，则比较他们是否是同一个对象，如果都指向同一个对象，则返回 true，否则返回 false；

7. 在全等和全不等的判断上，只有值和类型都相等，才返回 true，否则返回 false。


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


### 逻辑运算符

逻辑运算符通常用于布尔值的操作，一般和关系运算符配合使用，有三个逻辑运算符：

逻辑与（AND）：`&&`

    num1 && num2
    true    true    true
    true    false   false
    false   true    false
    false   false   false

如果两边的操作数有一个操作数不是布尔值的情况下，与运算就不一定返回布尔值，此时，遵循已下规则：

1. 第一个操作数是对象，则返回第二个操作数；

2. 第二个操作数是对象，则第一个操作数返回 true，才返回第二个操作数，否则返回 false；

3. 有一个操作数是 null，则返回 null；

4. 有一个操作数是 undefined，则返回 undefined。

逻辑或（OR）：`||`

    num1 || num2
    true    true     true
    true    false    true
    false   true     true
    false   false    false

如果两边的操作数有一个操作数不是布尔值的情况下，逻辑与运算就不一定返回布尔值，此时，遵循已下规则：

1. 第一个操作数是对象，则返回第一个操作数；

2. 第一个操作数的求值结果为 false，则返回第二个操作数；

3. 两个操作数都是对象，则返回第一个操作数；

4. 两个操作数都是 null，则返回 null；

5. 两个操作数都是 NaN，则返回 NaN；

6. 两个操作数都是 undefined，则返回 undefined。

逻辑非（NOT）：`!`

逻辑非参考： [JavaScript数据判断][2]

逻辑非运算符可以用于任何值。无论这个值是什么数据类型，这个运算符都会返回一个布尔值。它的流程是：先将这个值转换成布尔值，然后取反，规则如下：

1. 操作数是一个对象，返回 false；

2. 操作数是一个空字符串，返回 true；

3. 操作数是一个非空字符串，返回 false；

4. 操作数是数值 0，返回 true；

5. 操作数是任意非 0 数值（包括 Infinity），false；

6. 操作数是 null，返回 true；

7. 操作数是 NaN，返回 true；

8. 操作数是 undefined，返回 true。

不过，逻辑非也比较特殊。可以更好的记忆：[!的判断][3]

    var xzavier = !(123 > 12); //false
    var xzavier = !{}; //false
    var xzavier = !''; //true
    var xzavier = !'xzavier'; //false
    var xzavier = !0; //true
    var xzavier = !123; //false
    var xzavier = !null; //true
    var xzavier = !NaN; //true
    var xzavier = !undefined; //true


### 位运算符

在一般的应用中，我们基本上用不到位运算符。位非 NOT `~`、位与 AND `&`、位或 OR `|`、位异或 XOR `^`、左移 `<<`、有符号右移 `>>`、无符号右移 `>>>`。

    var xzavier = ~123; //-124
    var xzavier = 123 & 3; //3
    var xzavier = 123 | 3; //123
    var xzavier = 123 << 3; //984
    var xzavier = 123 >> 3; //15
    var xzavier = 123 >>> 3; //15

过程勉强看一下哈，不想写很多0101，所以写在纸上O(∩_∩)O~

![图片描述][4]

### 赋值运算符

    var xzavier = 123; //把123赋值给xzavier变量
    xzavier = xzavier +123; //246

更多类似赋值运算符

1. 乘/赋 `*=`

2. 除/赋 `/=`

3. 取余/赋 `%=`

4. 加/赋 `+=`

5. 减/赋 `-=`

6. 左移/赋 `<<=`

7. 有符号右移/赋 `>>=`

8. 无符号右移/赋 `>>>=`

### 三目运算符

    function absN(xzavier) {
        return xzavier > 0 ? xzavier : -xzavier;
    }
    absN(-123);  //123
    absN(123);  //123

### 逗号运算符

逗号运算符用于对两个表达式求值，并返回后一个表达式的值。

    'xza', 'vier' // "vier"
    
    var x = 0;
    var y = (x++, 10);  
    x // 1 
    y // 10

    
### 运算符优先级

    . [] ()                          对象成员存取、数组下标、函数调用等
    ++ -- ~ ! delete new typeof void 一元运算符
    乘法 / %                          乘法、除法、去模
    加法 - +                          加法、减法、字符串连接
    << >> >>>                        位移
    < <= > >= instanceof             关系比较、检测类实例
    == != === !==                    恒等(全等)
    &                                位与
    ^                                位异或
    |                                位或
    &&                               逻辑与
    ||                               逻辑或
    ?:                               三元条件
    = x=                             赋值、运算赋值
    ,                                多重赋值、数组元素分隔符
    圆括号()可以用来提高运算的优先级，因为它的优先级是最高的，即圆括号中的表达式会第一个运算。
    
几个有意思的等式：

    [1,2] + [3,4] == "1,23,4";  //true
    [4,[3,2]][5][0] == 3;  //true
    ++[[]][+[]]+[+[]] == '10';  //true

今天好天气，打篮球去咯。代码，篮球，生活...


  [1]: https://segmentfault.com/a/1190000005863067
  [2]: https://segmentfault.com/a/1190000006672446
  [3]: https://segmentfault.com/a/1190000006672446#articleHeader2
  [4]: https://sfault-image.b0.upaiyun.com/236/463/2364636534-57c577652fdb2_articlex
  [5]: https://segmentfault.com/a/1190000005863067
