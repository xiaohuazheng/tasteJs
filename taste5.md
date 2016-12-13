##JavaScript-正则表达式
###正则表达式简述

正则表达式(regular expression)描述了一种字符串匹配模式，可以用来检查一个字符串是否含有某类字符串、将匹配的字符串做替换或者从某个字符串中取出符合某个条件的字符串等。ECMAScript的RegExp对象表示正则表达式，而String 和RegExp 都定义了使用正则表达式进行强大的模式匹配和文本检索与替换的函数。

###正则表达式修饰符

    参数       含义                           备注
    i       忽略大小写
    g       全局匹配
    m       多行匹配
    u       正确处理四个字节的UTF-16编码       ES6新增
    y       确保匹配必须从剩余的第一个位置开始   ES6新增
    
###正则表达式创建
创建正则表达式和创建字符串类似，创建正则表达式提供了两种方法，一种是采用new
运算符，另一个是采用字面量方式。

    var xzavier = new RegExp('xzavier');       //第一个参数字符串
    var xzavier = new RegExp('xzavier', 'ig'); //第二个参数可选修饰符
    var xzavier = /xzavier/;                   //直接用两个反斜杠
    var xzavier = /xzavier/ig;                 //第二个斜杠后面加上的是修饰符

和对象数组等一样，我们一致推崇使用字面量的方式。简便快捷。但是也有必须使用new的方式，
当你的正则表达式中含有变量的时候：

    var reg = 'v';
    var pattern = new RegExp('xza' + reg + 'ier'); // 这时候就不能使用字面量的方式了

###正则表达式方法

RegExp 对象的test() 方法在字符串中查找是否存在指定的正则表达式并返回布尔值，如果存在则返回true，不存在则返回false。 

    var pattern = new RegExp('xzavier', 'i');  //正则模式，不区分大小写
    var pattern1 = /xzavier/i; //创建正则模式，不区分大小写
    var str = 'This is Xzavier!'; 
    console.log(pattern.test(str));  //true
    console.log(pattern1.test(str));  //true
    console.log(/xzavier/i.test(str));  //true
    
RegExp 对象的exec()方法也用于在字符串中查找指定正则表达式，如果exec()方法执行成
功，则返回包含该查找字符串的相关信息数组。如果执行失败，则返回null。

    var pattern = new RegExp('xzavier', 'i'); 
    var pattern1 = /xzavier/i; 
    var str = 'This is Xzavier!'; 
    console.log(pattern.exec(str));  //["Xzavier"]
    console.log(pattern1.exec(str));  //["Xzavier"]
    console.log(/xzavier1/i.exec(str));  //null

String 对象也提供了4 个使用正则表达式的方法。
match(pattern) 返回pattern中的匹配的字符串或null:

    var pattern = /xzavier/ig;   //全局匹配
    var str = 'This is Xzavier, this is Xzavier too.'; 
    console.log(str.match(pattern));  //["Xzavier", "Xzavier"]
    console.log(str.match(pattern).length);  //2
    console.log('javascript'.match(/xzavier/ig));  //null

replace(pattern, replacement) 用replacement替换pattern:

    var pattern = /xzavier/ig;  
    var str = 'This is Xzavier, this is Xzavier too.'; 
    console.log(str.replace(pattern, 'JavaScript'));  //This is JavaScript, this is JavaScript too.

search(pattern) 返回字符串中pattern开始位置,查找到返回位置，否则返回-1:

    var pattern = /xzavier/i;  
    var str = 'This is Xzavier, this is Xzavier too.'; 
    var str1 = 'This is JavaScript, this is JavaScript too.'; 
    console.log(str.search(pattern));   //8
    console.log(str1.search(pattern));   //-1

split(pattern) 返回字符串按指定pattern 拆分的数组:

    var pattern = / /ig;  
    var str = 'This is Xzavier, this is Xzavier too.'; 
    console.log(str.split(pattern));   //["This", "is", "Xzavier,", "this", "is", "Xzavier", "too."]

###RegExp对象的静态属性

    属性              短名      含义
    input             $_    当前被匹配的字符串
    lastMatch         $&    最后一个匹配字符串
    lastParen         $+    最后一对圆括号内的匹配子串
    leftContext       $`    最后一次匹配前的子串
    rightContext      $'    在上次匹配之后的子串

    var pattern = /(x)zavier/;
    var str = 'This is xzavier！';
    pattern.test(str); 
    console.log(RegExp.input); //This is xzavier！
    console.log(RegExp.leftContext); //This is 
    console.log(RegExp.rightContext); //！
    console.log(RegExp.lastMatch); //xzavier
    console.log(RegExp.lastParen); //x

###RegExp对象的实例属性

    属性                   含义
    global       Boolean值，表示g是否已设置
    ignoreCase   Boolean 值，表示i 是否已设置
    lastIndex    整数，代表下次匹配将从哪里字符位置开始
    multiline    Boolean值，表示m是否已设置
    Source       正则表达式的源字符串形式
    
    var pattern = /xzavier/ig;
    console.log(pattern.global); //true，是否设置了全局
    console.log(pattern.ignoreCase); //true，是否设置了忽略大小写
    console.log(pattern.multiline); //false，是否设置了换行
    console.log(pattern.lastIndex); //0，下次匹配位置
    console.log(pattern.source); //xzavier，正则表达式的源字符串

###正则表达式元字符

####字符类：单个字符和数字	

<img src="https://sfault-image.b0.upaiyun.com/212/892/2128921545-578b6391d71e4_articlex">

注意： /\\./ 和 /[.]/ 只能匹配'.',不匹配通配符

`\\` 引用符，用来将这里列出的这些元字符当作普通的字符来进行匹配。如.用来匹配点字符，而不是任何字符的通配符。


`[ ]`,`[c1-c2]`,`[^c1-c2]`字符组，匹配括号中的任何一个字符,并不是要全部匹配。如/x[zav]e/匹配xze、xae和xve，但是不匹配xxe。如/[0-9]/可以匹配任何数字字符；如/[A-Za-z]/可以匹配任何大小写字母。如正则表达式`[^269A-Z]` 将匹配除了2、6、9和所有大写字母之外的任何字符。

对于这两个操作符，特殊符号没有绝对规律，倒是特殊字母匹配符还是有规律的，见下

    'love.'.replace(/./, '');  //"ove."  通配
    'love.'.replace(/\./, '');  //"love"  点
    'love.'.replace(/[.]/, ''); //"love"  点
    'love.'.replace(/[\.]/, ''); //"love"  点
    
    但是：
    
    'lo v^se.'.replace(/\^/, ''); //"lo vse."  匹配^
    'lo v^se.'.replace(/[^]/, '');  //"o v^se." 匹配开始去了，并没有匹配^
    'lo v^se.'.replace(/[\^]/, ''); //"lo vse." 要加一个这样才匹配^
    
    
    'lo vse.'.replace(/\s/, '');  //"lovse." 匹配空格
    'lo vse.'.replace(/[s]/, ''); //"lo ve."  匹配字母
    'lo vse.'.replace(/[\s]/, '');  //"lovse."  要加一个\才匹配空格
    
    '    lovte.'.replace(/\t/, '');  //"lovte. 匹配制表符
    '    lovte.'.replace(/[t]/, ''); //"    love."  匹配字母
    '    lovte.'.replace(/[\t]/, '');  //"lovte."  要加一个\才匹配制表符

####字符类：空白字符	

<img src="https://sfault-image.b0.upaiyun.com/187/869/187869806-578b6b2b33b59_articlex">

\\b是匹配字符串开头结尾及空格回车等的位置,单词边界, 不会匹配空格符本身。\\s则是匹配空白字符本身、空格符本身、换行符本身。

####字符类：锚字符

<img src="https://sfault-image.b0.upaiyun.com/417/611/4176112660-578b6b59e2e70_articlex">

####字符类：重复字符

<img src="https://sfault-image.b0.upaiyun.com/173/495/1734955041-578b6b8eee0b7_articlex">

####字符类：替代字符

    a|b|c    匹配a或b或c中的任意一个

####字符类：记录字符

    $n     与 regexp 中的第 n(1~99) 个子表达式相匹配的文本
    $&     表示与 regexp 相匹配的子串
    $`	 位于匹配子串左侧的文本
    $'	 位于匹配子串右侧的文本
    $$	 直接量符号

    'you are beautiful'.replace(/beautiful/g, 'so $&');  //"you are so beautiful"
    'leftright'.replace(/right/, '$`');  //"leftleft"
    'leftright'.replace(/left/, "$'");  //"rightright"

###贪婪模式和惰性模式

    贪婪  惰性
    '+'   +?
    ?     ??
    \*    *?
    {n}   {n}?
    {n,}  {n,}?
    {n,m} {n,m}?

    var pattern = /[a-z]+/; //贪婪匹配，全部替换
    var str = 'qqqwwweee';
    var result = str.replace(pattern, 'xxx');
    console.log(result);  //xxx

    var pattern = /[a-z]+?/; //?号关闭了贪婪匹配，只替换了第一个
    var str = 'qqqwwweee';
    var result = str.replace(pattern, 'xxx');
    console.log(result);  //xxxqqwwweee

###断言

先行断言： x(?=y)，找到x后面紧跟着y的位置，如果找到则匹配这个位置

    var pattern = /(xza(?=vier))/; //xza后面必须跟着vier才能捕获
    var str = 'hello,xzavier';
    console.log(pattern.test(str));  //true

先行否定断言 x(?!y)，找到x后面不是y的位置，如果找到则匹配这个位置

    var pattern = /(xza(?!vier))/; //xza后面必须跟着的不是vier才能捕获
    var str = 'hello,xzaqqqvier';
    console.log(pattern.test(str));  //true

可惜，JavaScript不支持 后行断言 和 后行否定断言。
当然，现在不支持，不代表未来不支持。虽然外面最新的ES6也没有推出正式的标准，但是已经有了[提案][1]，ES7中应该会推出标准实现 后行断言 和 后行否定断言。
届时我们可能就能用到这两个功能，这样的代码了：

    var pattern = /(?=xza)vier/; //vier前面必须是xza才能捕获
    var str = 'hello,xzavier';
    console.log(pattern.test(str));  //true
    
    var pattern = /(?!xza)vier)/; //vier前面必须不是xza才能捕获
    var str = 'hello,xzaqqqvier';
    console.log(pattern.test(str));  //true

这代码现在是不能使用的，但是我们想要实现类似的功能，用别的方式，多写两行代码也就实现了。

###捕获性分组和非捕获性分组

捕获性分组

    var pattern = /(\d+)([a-z])/; //捕获性分组
    var str = '123abc';
    console.log(pattern.exec(str));  //["123a", "123", "a"]

非捕获性分组  格式: (?:x) 

    var pattern = /(\d+)(?:[a-z])/; //非捕获性分组
    var str = '123abc';
    console.log(pattern.exec(str));  //["123a", "123"]

###常用正则表达式

亲测有效：

匹配中文字符： `[\u4e00-\u9fa5]`

匹配Email地址：`\w[-\w.+]*@([A-Za-z0-9][-A-Za-z0-9]+\.)+[A-Za-z]{2,14}/`

去除首尾空白：`/(^\s*)|(\s*$)/g`

去除多余空格：`/\s/g`

身份证：`\d{17}[\d|x]|\d{15}`

ip地址：`\d+\.\d+\.\d+\.\d+`

网址URL： `^((https|http|ftp|rtsp|mms)?:\/\/)[^\s]+`

QQ号：`[1-9]{4,}`

数字串千分法：`"1234567890".replace(/(\d)(?=(\d{3})+$)/g, "$1,");//"1,234,567,890"`

判断手机app内置浏览器：

    var ua = navigator.userAgent.toLowerCase(),
        isWx = /microMessenger/i.test(ua),
        isQQ = /\s+qq\//ig.test(ua),
        isQZone = /qzone/i.test(ua),
        isWeibo = /weibo/i.test(ua);

首字母大写：

    str = "hello woRld";
    String.prototype.initCap = function () {
       return this.toLowerCase().replace(/(?:^|\s)[a-z]/g, function (s) {
          return s.toUpperCase();
       });
    };
    console.log(str.initCap());  //Hello World

"yyyy-mm-dd" 格式的日期校验(平闰年)：

    function testDate(str) {
        var reg = /^(?:(?!0000)[0-9]{4}-(?:(?:0[1-9]|1[0-2])-(?:0[1-9]|1[0-9]|2[0-8])|(?:0[13-9]|1[0-2])-(?:29|30)|(?:0[13578]|1[02])-31)|(?:[0-9]{2}(?:0[48]|[2468][048]|[13579][26])|(?:0[48]|[2468][048]|[13579][26])00)-02-29)$/;
    	return reg.test(str);
    }
    testDate('2016-03-12'); //true
    testDate('2016-23-12'); //false
    testDate('2016-02-29'); //true
    testDate('2017-02-29'); //false


延伸阅读：[正则表达式的扩展][2]

正则测试：[在线正则表达式测试][3]


  [1]: https://github.com/goyakin/es-regexp-lookbehind
  [2]: http://es6.ruanyifeng.com/#docs/regex
  [3]: http://tool.oschina.net/regex/