#温故js系列（1）-基本数据类型和引用数据类型判断&存储访问&类型转换

##数据类型定义
###数据类型分类

基本数据类型：`String,boolean,Number,Symbol（ES6新增）,Undefined, Null`

引用数据类型：`Object`

基本数据类型中有两个为特殊数据类型： `null, undefined` 

js的常见内置对象：`Date,Array,Math,Number,Boolean,String,Array,RegExp,Error,Function...`



###数据类型访问&&复制
基本数据类型：基本数据类型值指保存在栈内存中的简单数据段。访问方式是按值访问。

    var a = 1;

![图片描述][1]

操作的是变量实际保存的值。
    a = 2;

![图片描述][2]


基本类型变量的复制：从一个变量向一个变量复制时，会在栈中创建一个新值，然后把值复制到为新变量分配的位置上。

    var b = a;

![图片描述][5]

    b = 2;

![图片描述][6]

引用数据类型：引用数据类型值指保存在堆内存中的对象。也就是，变量中保存的实际上的只是一个指针，这个指针指向内存中的另一个位置，该位置保存着对象。访问方式是按引用访问。

    var a = new Object();

![图片描述][3]

当操作时，需要先从栈中读取内存地址，然后再延指针找到保存在堆内存中的值再操作。

    a.name = 'xz';
![图片描述][4]

引用类型变量的复制：复制的是存储在栈中的指针，将指针复制到栈中未新变量分配的空间中，而这个指针副本和原指针指向存储在堆中的同一个对象；复制操作结束后，两个变量实际上将引用同一个对象。因此，在使用时，改变其中的一个变量的值，将影响另一个变量。

    var b = a;

![图片描述][7]

    b.sex = 'boy';

![图片描述][8]

漏画了，差一条指针。b的引用指针也指向`object{sex:'boy'}`

    b.sex;  //'boy'   a.name; //'boy'
    
###堆&栈
两者都是存放临时数据的地方。

栈是先进后出的，就像一个桶，后进去的先出来，它下面本来有的东西要等其他出来之后才能出来。

堆是在程序运行时，而不是在程序编译时，申请某个大小的内存空间。即动态分配内存，对其访问和对一般内存的访问没有区别。对于堆，我们可以随心所欲的进行增加变量和删除变量，不用遵循次序。

栈区（stack） 由编译器自动分配释放   ，存放函数的参数值，局部变量的值等。 

堆区（heap）  一般由程序员分配释放，若程序员不释放，程序结束时可能由OS回收。 

堆（数据结构）：堆可以被看成是一棵树，如：堆排序； 

栈（数据结构）：一种先进后出的数据结构。

##数据类型检测
###Typeof
typeof操作符是检测基本类型的最佳工具。

    "undefined" — 未定义

    "boolean"   — 布尔值

    "string"    — 字符串

    "number"    — 数值

    "object"    — 对象或null

    "function"  — 函数

###Instanceof
instanceof用于检测引用类型，可以检测到它是什么类型的实例。

instanceof 检测一个对象A是不是另一个对象B的实例的原理是：查看对象B的prototype指向的对象是否在对象A的[[prototype]]链上。如果在，则返回true,如果不在则返回false。不过有一个特殊的情况，当对象B的prototype为null将会报错(类似于空指针异常)。

    var sXzaver = new String("Xzavier"); 
    console.log(sXzaver instanceof String);   //  "true"
    var aXzaver = [1,2,3]; 
    console.log(aXzaver instanceof Array);   //  "true"
    检测数组在ECMA Script5中定义了一个新方法Array.isArray()
###Constructor
constructor属性返回对创建此对象的数组函数的引用。可以用于检测自定义类型。

    'xz'.constructor == String // true
    (123).constructor == Number // true
    (true).constructor == Boolean // true
    [1,2].constructor == Array // true
    ({name:'xz'}).constructor == Object // true
    (function(){}).constructor == Function // true
    (new Date()).constructor == Date // true
    (Symbol()).constructor == Symbol // true
    (/xz/).constructor == RegExp // true

constructor不适用于null和undefined。除了这些原生的，constructor还可验证自定义类型。

    function Xzavier(){}
    var xz = new Xzavier();
    xz.constructor == Xzavier;  // true 
   
###Object.prototype.toString.call(obj)
推荐使用：Object.prototype.toString.call(obj)

    原理：调用从Object继承来的原始的toString()方法
    
    Object.prototype.toString.call('xz'); //"[object String]"
    Object.prototype.toString.call(123);  //"[object Number]"
    Object.prototype.toString.call(true); //"[object Boolean]"
    Object.prototype.toString.call([1,2]); //"[object Array]"
    Object.prototype.toString.call({name:'xz'}); //"[object Object]"
    Object.prototype.toString.call(function(){}); //"[object Function]"
    Object.prototype.toString.call(null); //"[object Null]"
    Object.prototype.toString.call(undefined); //"[object Undefined]"
    Object.prototype.toString.call(); //"[object Undefined]"
    Object.prototype.toString.call(new Date()); //"[object Date]"
    Object.prototype.toString.call(/xz/);  //"[object RegExp]"
    Object.prototype.toString.call(Symbol()); //"[object Symbol]"
    
    var obj = {name:"Xzavier", age:23};
    var a = [1,2,3];
    
    function isType(obj) {
        return Object.prototype.toString.call(obj).slice(8, -1);
    }
    isType(obj);  // "Object" 
    isType(a)  // "Array"  
    
##数据类型转换
###隐式转换

    undefined == null;  // true   
    1 == true;  // true  
    2 == true;  // false  
    0 == false;  // true
    0 == '';  // true   
    NaN == NaN;  // false  NaN不等于任何值
    [] == false;  // true  
    [] == ![];  // true
    '6' - '3'  // 3
    1234 + 'abcd' // "1234abcd"

1.undefined与null相等，但不恒等（===）

2.一个是number一个是string时，会尝试将string转换为number

3.隐式转换将boolean转换为number，0或1

4.隐式转换将Object转换成number或string，取决于另外一个对比量的类型

5.对于0、空字符串的判断，建议使用 “===” 。

6.“===”会先判断两边的值类型，类型不匹配时为false。

###显示转换
显示转换一般指使用Number、String和Boolean三个构造函数，手动将各种类型的值，转换成数字、字符串或者布尔值。
####Number：

    Number('1234') // 1234
    Number('1234abcd') // NaN
    Number('') // 0
    Number(true) // 1
    Number(null) // 0
    Number(undefined) // NaN
    
####String：

    String(1234)  // "1234"
    String('abcd')  // "abcd"
    String(true)  // "true"
    String(undefined) // "undefined"
    String(null)  // "null"
    
####Boolean:

    Boolean(0)  // false
    Boolean(undefined)  // false
    Boolean(null)  // false
    Boolean(NaN)  // false
    Boolean('')  // false
使用总，!!相当于Boolean：

    !!'foo';   // true
    !!'';      // false
    !!'0';     // true
    !!'1';     // true
    !!'-1'     // true
    !!{};      // true
    !!true;    // true


Number、String、Boolean转换对象时主要使用了对象内部的valueOf和toString方法进行转换。

####Number转换对象：
1.先调用对象自身的valueOf方法。如果返回原始类型的值，则直接对该值使用Number函数，返回结果。

2.如果valueOf返回的还是对象，继续调用对象自身的toString方法。如果toString返回原始类型的值，则对该值使用Number函数，返回结果。

3.如果toString返回的还是对象，报错。

    Number([1]); //1
    转换演变：
    [1].valueOf(); // [1];
    [1].toString(); // '1';
    Number('1'); //1
####String转换对象
1.先调用对象自身的toString方法。如果返回原始类型的值，则对该值使用String函数，返回结果。

2.如果toString返回的是对象，继续调用valueOf方法。如果valueOf返回原始类型的值，则对该值使用String函数，返回结果。

3.如果valueOf返回的还是对象，报错。

    String([1,2]) //"1,2"
    转化演变：
    [1,2].toString();  //"1,2"
    String("1,2");  //"1,2"
####Boolean转换对象
Boolean转换对象很特别，除了以下六个值转换为false，其他都为true

    undefined  null  false  0(包括+0和-0)  NaN  空字符串('')
    Boolean(undefined)   //false
    Boolean(null)        //false
    Boolean(false)       //false
    Boolean(0)           //false
    Boolean(NaN)         //false
    Boolean('')          //false
    
    Boolean([])          //true
    Boolean({})          //true
    Boolean(new Date())  //true



写写博客打打球...要代码，要篮球，更要生活。。。


  [1]: https://sfault-image.b0.upaiyun.com/130/011/1300118440-57c4093986c6d_articlex
  [2]: https://sfault-image.b0.upaiyun.com/327/696/3276962861-57c4098d30fec_articlex
  [3]: https://sfault-image.b0.upaiyun.com/214/291/2142910985-57c40b2ed77e8_articlex
  [4]: https://sfault-image.b0.upaiyun.com/329/435/3294358084-57c40b4ec3602_articlex
  [5]: https://sfault-image.b0.upaiyun.com/955/852/955852881-57c409c0cd3ad_articlex
  [6]: https://sfault-image.b0.upaiyun.com/370/347/3703477448-57c40ac4d82a5_articlex
  [7]: https://sfault-image.b0.upaiyun.com/260/647/2606476633-57c40b8cd72b3_articlex
  [8]: https://sfault-image.b0.upaiyun.com/250/178/2501783615-57c40c141541f_articlex
  [9]: http://javascript.ruanyifeng.com/grammar/conversion.html
  [10]: http://www.2ality.com/2012/01/object-plus-object.html