## Array对象

之前一直在[温故js系列][1]，希望能够知新，不过最近应业务要求，在做移动WEB，需求大，任务多。所以，只有像现在闲着的时候才能继续温故js了。祖国母亲的生日，祝祖国繁荣昌盛O(∩_∩)O~ 不知道有多少程序猿宅在家里，多少程序猿堵在路上...

在 JavaScript 中 Array 是一个用来构造数组的全局对象，它是一个高阶的类似有序列表的对象，是JavaScript内置对象里非常重要的一个。

创建数组：

### 数组字面量

    var arr = []; 
    var arr = [1, 2, 3];
    var arr = [[1],2,[2,[123]]];        

### 数组构造函数

    var arr = new Array();  //[]
    var arr = new Array(1,2,3); //[1, 2, 3]
    var arr = new Array(3);  //[undefined,undefined,undefined] 参数3为数组length
    var arr = new Array([1],2,[2,[123]]); //[[1],2,[2,[123]]]

建议使用数组字面量方式，性能好，代码少，简洁，毕竟代码少。

## Array属性

### `length`属性

length属性返回数组的长度，是一个可变属性，表示一个数组中的元素个数。
数组的索引由0开始，所以一个数组的最前和最后的值为限分别是：`arr[0]`和`arr[arr.length-1]`

    var arr = [1,2,3];
    arr.length // 3
    arr.length = 2; //改变数组length，从后往前截取
    arr // [1,2],此时arr.length为2。所以平时我们可以用length来操作数组（删除，添加）
    arr.length = 4;
    arr // // [1,2,undefined,undefined],此时arr.length为2,后面填充undefined

### `prototype`属性

`prototype`属性返回对象类型原型的引用，所有的数组实例都继承了这个属性，所有的数组方法都定义在 Array.prototype 身上。一般来说，我们经常用prototype属性来扩展数组方法：

    //给数组添加个方法，返回数组中的最大值
    Array.prototype.max = function() {
        return Math.max.apply(null,this); 
    }
    [1,2,3,4].max(); //4
    
    //给数组添加个方法，给数组去重
    Array.prototype.unique = function() {
        return this.filter((item, index, arr) => arr.indexOf(item) === index);
    }
    [11,2,1,1,2,3,1,2,4,5,23,2].unique(); //[11, 2, 1, 3, 4, 5, 23]

数组去重：[数组去重演化][2]

###`constructor`属性
`constructor`属性返回创建对象的函数，即构造函数。如下：

    var arr = [1,2,3];
    arr.constructor  //function Array() { [native code] }
    arr.constructor === Array  //true  即 new Array
    var a = new Array();
    a.constructor === Array  //true
对于数组来说，这个属性还是罕见使用的。
##数组判断
###Array.isArray()
`Array.isArray()` 方法用来判断某个值是否为Array。如果是，则返回 true，否则返回 false

    Array.isArray([]);  //true
    Array.isArray([1,2,3]);  //true
    Array.isArray(new Array());  //true
    Array.isArray();  //false
    Array.isArray({});  //false
    Array.isArray(123);  //false
    Array.isArray('xzavier');  //false
###利用属性自己写方法
`Array.isArray()`在ES5之前不支持，就自己写。不过现在都到ES6了，可以不管了。
    Array.prototype.isArray = Array.prototype.isArray || function() {
        return Object.prototype.toString.call(this) === "[object Array]";
    }
    [1,2,3].isArray(); //true
##数组遍历
关于这几个遍历语句的详细讲解可参考：[温故js系列（8）-流程控制][3]
###经典的`for`

    for (var index = 0; index < arr.length; index++) {
        console.log(arr[index]);
    }
这种写法很经典，就是语句多，但是性能好。
###ES5的`forEach`

    arr.forEach(function (value) {
        console.log(value);
    });
这种写法简洁，但这种方法也有一个小缺陷：你不能使用break语句中断循环，也不能使用return语句返回到外层函数。
###不建议的`for-in`    

    for (var i in arr) { 
        console.log(arr[i]);
    }
`for-in`是为普通对象设计的，你可以遍历得到字符串类型的键，因此不适用于数组遍历。但是它能遍历数组，作用于数组的`for-in`循环体除了遍历数组元素外，还会遍历自定义属性。举个例子，如果你的数组中有一个可枚举属性arr.name，循环将额外执行一次，遍历到名为“name”的索引。就连数组原型链上的属性都能被访问到。所以，不建议使用。
###ES6的`for-of`

    for (var value of arr) {
        console.log(value); // 1 2 3 
    }
这是最简洁、最直接的遍历数组元素的语法。这个方法避开了for-in循环的所有缺陷。与forEach()不同的是，它可以正确响应break、continue和return语句。

    for (var value of arr) {
        if(value == 2){break;}
        console.log(value);  //1
    }
##数组方法细说
###`splice`插入、删除、替换
splice() 方法可以插入、删除或替换数组的元素，注意：`同时改变了原数组`。

1.删除-删除元素，传两个参数，要删除第一项的位置和第二个要删除的项数 
2.插入-向数组指定位置插入任意项元素。三个参数，第一个参数（位置），第二个参数（0），第三个参数（插入的项）
3.替换-向数组指定位置插入任意项元素，同时删除任意数量的项，三个参数。第一个参数（起始位置），第二个参数（删除的项数），第三个参数（插入任意数量的项）

    var arr = ["q","w","e"]; 
    //删除 
    var removed = arr.splice(1,1); 
    console.log(arr); //q,e  已被改变
    console.log(removed); //w ,返回删除的项 
    //插入 
    var insert = arr.splice(0,0,"r"); //从第0个位置开始插入 
    console.log(insert); //返回空数组 
    console.log(arr); //r,q,e 
    //替换 
    var replace = arr.splice(1,1,"t"); //删除一项，插入一项 
    console.log(arr); //r,t,e
    console.log(replace); //q,返回删除的项 
###`sort` 排序
sort() 方法对数组的元素做原地的排序，并返回这个数组。 
    var arr = [1,2,4,3,1,1,2];
    console.log(arr.sort());//[1, 1, 1, 2, 2, 3, 4]
    
    然而：
    var arr = [1,2,10,4,3,1,1,2];
    console.log(arr.sort());//[1, 1, 1, 10, 2, 2, 3, 4]
这是因为`sort`排序可能是不稳定的，默认按照字符串的Unicode码位点排序。

但是，`sort()`方法接受一个参数，这个参数是一个函数，可选，用来指定按某种顺序进行排列的函数。如果省略，元素按照转换为的字符串的诸个字符的Unicode位点进行排序。

    var arr = [1,2,10,4,3,1,1,2];
    console.log(arr.sort(function(a,b){
        return a-b; 
    })); // [1, 1, 1, 2, 2, 3, 4, 10]
这个函数就是我们自己控制了，我们想要什么样的排序就改变这个参数函数的逻辑即可。
###`slice`截取、转化arguments伪数组
`slice()`方法可从已有的数组中返回选定的元素数组。不会修改原数组，只会会浅复制数组的一部分到一个新的数组，并返回这个新数组。

语法：`arrayObject.slice(start,end)` 参数可为正负。

    start   必需。规定从何处开始选取。如果是负数，那么它规定从数组尾部开始算起的位置。也就是说，-1 指最后一个元素，-2 指倒数第二个元素，以此类推。
    end   可选。规定从何处结束选取。该参数是数组片断结束处的数组下标。如果没有指定该参数，那么切分的数组包含从 start 到数组结束的所有元素。
             如果这个参数是负数，那么它规定的是从数组尾部开始算起的元素。

    var arr = [1,2,3,4,5];
    arr.slice(0,3);    //  [1,2,3]
    arr.slice(3);      //  [4,5]
    arr.slice(1,-1);   //  [2,3,4]
    arr.slice(-3,-2);  //  [3]
    var arr1 = arr.slice(0); //返回数组的拷贝数组，是一个新的数组，不是赋值指向
`slice`方法经常用来截取一个数组，不过它更常用在将伪数组转化为真数组。平时我们的函数传的参数`arguments`是一个伪数组，很多数组的方法不能使用，我们就需要将伪数组转化为真数组。

    function test() {
        var arr = arguments;
        arr.push('xza');
        console.log(JSON.stringify(arr));
    }
    test(1,2,3);  //arr.push is not a function(…) 因为伪数组没有push方法
    
    转换后：
    function test() {
        var arr = Array.prototype.slice.call(arguments);
        arr.push('xza');
        console.log(JSON.stringify(arr));
    }
    test(1,2,3); //[1,2,3,"xza"]
###`filter` 过滤
`filter()` 方法使用指定的函数测试所有元素，并创建一个包含所有通过测试的元素的新数组。简单来说就是对数组进行过滤，返回一个过滤过的数组。

语法：`array.filter(function(currentValue,index,arr), thisValue)`

    function(currentValue, index,arr)   必须。函数，数组中的每个元素都会执行这个函数
    
    函数的三个参数:currentValue必须，当前元素的值； index可选，当期元素的索引值； arr可选，当期元素属于的数组对象
    thisValue   可选。对象作为该执行回调时使用，传递给函数，用作 "this" 的值。如果省略了 thisValue ，"this" 的值为 "undefined"

    //用filter给数组添加个方法，给数组去重
    Array.prototype.unique = function() {
        return this.filter((item, index, arr) => arr.indexOf(item) === index);
    }
    [11,2,1,1,2,3,1,2,4,5,23,2].unique(); //[11, 2, 1, 3, 4, 5, 23]
`filter()` 不会对空数组进行检测，不会改变原始数组。
###`concat` 连接数组

    var arr1 = [1,2,3];
    var arr2 = [4,5,6];
    var arr3 = arr1.concat(arr2);  //[1, 2, 3, 4, 5, 6]
    arr3.concat(7); //[1, 2, 3, 4, 5, 6, 7]
我们平时都是这么使用的，如果需要连接两个数组的元素时，中间插元素，可以
    var arr3 = arr1.concat('xzavier', arr2); //[1, 2, 3, "xzavier", 4, 5, 6]
不加参数相当于拷贝，返回数组的拷贝数组，是一个新的数组，并不是指向原来数组。
    var arr4 = arr1.concat(); //[1,2,3]
    var arr5 = arr1;
    arr4 === arr1; //false
    arr5 === arr1; //true
###插入删除
前面讲了个`splice`可以在数组的任何位置插入删除元素，这儿讲的是只能在首尾插入删除的方法，用起来也很方便。

在数组尾插入删除：
    push()方法可以接收任意数量的参数，把它们逐个添加到数组的末尾，并返回修改后数组的长度。
    pop()方法则从数组末尾移除最后一个元素，减少数组的length值，然后返回移除的元素。

    var arr  = [1,2,3];
    arr.push(4);  // 返回的length 4
    arr.pop();   //返回的删除值  4
    arr  //[1, 2, 3]
在数组头插入删除：
    unshift()方法为数组的前端添加一个元素
    shift()方法从数组前端移除一个元素
    
    var arr  = [1,2,3];
    arr.unshift(4);  // 返回的length 4
    arr.shift();   //返回的删除值  4
    arr  //[1, 2, 3]
###其他方法

    方法                使用
    concat()         连接两个或更多的数组，并返回结果。
    join()         把数组的所有元素放入一个字符串。元素通过指定的分隔符进行分隔。
    reverse()       颠倒数组中元素的顺序。
    toString()     把数组转换为字符串，并返回结果。
    toLocaleString() 把数组转换为本地数组，并返回结果。
    valueOf()       返回数组对象的原始值
    map()            返回一个由原数组中的每个元素调用一个指定方法后的返回值组成的新数组。
    every()          测试数组的所有元素是否都通过了指定函数的测试。
    some()           测试数组中的某些元素是否通过了指定函数的测试。
小试：（欢迎补充和斧正问题，更多方法延伸阅读：[ES6数组的扩展][4]）
    ar arr = ['xzavier',123,'jser'];
    console.log(arr.valueOf());  //['xzavier',123,'jser']
    console.log(arr.toString());  //xzavier,123,jser
    console.log(arr.toLocaleString());  //xzavier,123,jser
    var arr = ['xzavier',123,'jser'];
    console.log(arr.join(','));  //xzavier,123,jser
    var arr = [1,2,3];
    console.log(arr.reverse());  //[3,2,1]
    var numbers = [1, 4, 9];
    var roots = numbers.map(Math.sqrt); //[1,2,3]
    numbers //[1,4,9]
    roots // [1,2,3]
    [2, 5, 1, 4, 3].some(function (element, index, array) {
        return (element >= 10);
    });  //false
    [2, 5, 1, 4, 13].some(function (element, index, array) {
        return (element >= 10);
    });  //true
    [2, 5, 1, 4, 13].every(function (element, index, array) {
        return (element >= 10);
    });  //false
    [2, 5, 1, 4, 13].every(function (element, index, array) {
        return (element >= 0);
    });  //true
###趣味探索
    [1,2] + [3,4] == "1,23,4";  //true
    ++[[]][+[]]+[+[]] == '10';  //true
    console.log ([] == ![]);    //true  


  [1]: https://segmentfault.com/blog/xzavier
  [2]: https://segmentfault.com/a/1190000006632291
  [3]: https://segmentfault.com/a/1190000006669505
  [4]: http://es6.ruanyifeng.com/#docs/array