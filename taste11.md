## JavaScript-BOM

BOM是browser object model的缩写，简称浏览器对象模型。它本身是没有标准的或者还没有哪个组织去标准它，所以，BOM缺乏标准。它提供了很多对象，并且每个对象都提供了很多方法与属性，用于访问浏览器的功能。

###window对象

BOM的核心对象是window，它表示浏览器的一个实例，主要用于管理窗口与窗口之间的通讯。window 对象处于JavaScript结构的最顶层，对于每个打开的窗口，系统都会自动为其定义window 对象。F12打开console,输入window,即可看到window的所有属性和方法。太多了，很多是不需要我们去深入了解的。

#### window 对象的属性

    属性                         含义
    closed                 当窗口关闭时为真
    defaultStatus          窗口底部状态栏显示的默认状态消息
    document               窗口中当前显示的文档对象
    frames                 窗口中的框架对象数组
    history                保存着窗口最近加载的URL
    length                 窗口中的框架数
    location               当前窗口的URL
    name                   窗口名
    offscreenBuffering     用于绘制新窗口内容并在完成后复制已存在的内容，控制屏幕更新
    opener                 打开当前窗口的窗口
    parent                 指向包含另一个窗口的窗口（由框架使用）
    screen                 显示屏幕相关信息，如高度、宽度（以像素为单位）
    self                   指示当前窗口。
    status                 描述由用户交互导致的状态栏的临时消息
    top                    包含特定窗口的最顶层窗口（由框架使用）
    window                 指示当前窗口，与self 等效

要验证这些属性，直接F12打开console，验证即可。

#### window 对象的方法

    方法                                 功能
    alert(text)                  创建一个警告对话框，显示一条信息
    blur()                       将焦点从窗口移除
    clearInterval(interval)      清除之前设置的定时器间隔
    clearTimeOut(timer)          清除之前设置的超时
    close()                      关闭窗口
    confirm()                    创建一个需要用户确认的对话框
    focus()                      将焦点移至窗口
    open(url,name,[options])     打开一个新窗口并返回新window 对象
    prompt(text,input)           创建一个对话框要求用户输入信息
    scroll(x,y)                  在窗口中滚动到一个像素点的位置
    moveBy(x,y)                  按照给定像素参数移动指定窗口,x,y代表水平位移量和垂直位移量
    moveTo(x,y)                  将窗口移动到指定的指定坐标(x,y)处
    resizeBy(x,y)                将当前窗口改变指定的大小(x,y)，当x、y的值大于0时为扩大，小于0时为缩小
    resizeTo(x,y)                将当前窗口改变大小，x、y分别为宽度和高度
    print()                      调出打印对话框
    find()                       调出查找对话框
    setInterval(func,time)       经过指定时间间执行代码
    clearInterval("id");         取消setInterval    
    setTimeout(func,time)        在定时器超过后执行代码
    clearTimeout("id");          取消还未执行的setTimeout

window下的属性和方法，可以使用window.属性、window.方法()或者直接属性、方法()的方式调用。下面简要介绍几个。

### window属性-窗口的位置和大小

窗口的位置，IE、Safari、Opera 和Chrome都提供了screenLeft 和screenTop 属性，分别用于表示窗口相对于屏幕左边和上边的位置。firefox 则在screenX 和screenY 属性中提供相同的窗口位置信息，Safari 和Chrome 也同时支持这两个属性。

    console.log(screenLeft);   //102, fireFox下输出screenLeft is not defined
    console.log(screenX);      //102

兼容：

    var leftX = (typeof screenLeft == 'number') ? screenLeft : screenX;
    var topY = (typeof screenTop == 'number') ? screenTop : screenY;

窗口页面大小，IE、Firefox、Safari、Opera 和Chrome 均为此提供了4 个属性：innerWidth和innerHeight，返回浏览器窗口本身的尺寸；outerWidth 和outerHeight，返回浏览器窗口本身及边框的尺寸。

    console.log(innerWidth);  //1366 页面宽度
    console.log(innerHeight); //681 页面高度
    console.log(outerWidth);  //1366 页面长度+边框
    console.log(outerHeight); //768 页面高度+边框
    //缩小窗口数据就会有变化

旧的IE版本没有提供当前浏览器窗口尺寸的属性。不过，DOM有提供相关的方法：

    document.documentElement.clientWidth
    document.documentElement.clientHeight

当然，这是标准模式下，还要兼容怪异模式：

    document.body.clientWidth; //非标准模式使用body
    document.body.clientHeight;

### window方法-系统对话框

浏览器通过alert()、confirm()和prompt()方法可以调用系统对话框向用户显示信息。

#### alert()

    alert('xzavier warning');

直接弹出弹框，弹框只有确定和关闭按钮。
一般用于用户交互时做出提示，不过这种方法的用户体验不是很好，现在都流行使用业务需要的用户体验好的弹窗。
另外，以前常用于调试代码，但现在基本不用了。因为它很烦，弹一个要关一个。如果调试代码后忘记关掉，还会给用户带来较差的体验。现在都用console，在控制台打印，研发人员可以很方便的看到调试信息，也不用去关闭弹窗之类的，就算一时疏忽忘记删掉调试代码，也不会影响到用户体验。

#### confirm()

    confirm('confirm or cancel');  

确定和取消弹框，按确定返回true，按取消或者关闭按钮返回false。

    if (confirm('confirm or cancel')) { 
        alert('you confirmed'); 
    } else {
        alert('you canceled');
    } 

#### prompt()

    prompt('please input a number', 0);

输入提示框，带两个参数，一个提示，一个值。按确定返回输入值，不填返回空，按取消或者关闭按钮返回null。

### window方法-新建窗口

window.open()方法可以导航到一个特定的URL，也可以打开一个新的浏览器窗口。它可以接受四个参数：
1.要加载的URL
2.窗口的名称或窗口目标
3.一个字符串参数，表示新窗口的长宽等属性值
4.一个表示新页面是否取代浏览器记录中当前加载页面的布尔值。

    open('https://segmentfault.com/blog/xzavier'); //新建页面并打开
    open('https://segmentfault.com/blog/xzavier','xzavier'); //新建页面并命名窗口并打开
    open('https://segmentfault.com/blog/xzavier','_parent'); //在本页窗口打开

target 属性：
blank - 在一个新的未命名的窗口载入文档
_self - 在相同的框架或窗口中载入目标文档
_parent - 把文档载入父窗口或包含了超链接引用的框架的框架集
_top - 把文档载入包含该超链接的窗口,取代任何当前正在窗口中显示的框架

字符串参数：

    属性         值             说明
    width       数值      新窗口的宽度 ，不能小于100
    height      数值      新窗口的高度，不能小于100
    top         数值      新窗口的Y坐标，不能是负值
    left        数值      新窗口的X坐标，不能是负值
    location   yes/no    是否在浏览器窗口中显示地址栏，不同浏览器默认值不同
    menubar    yes/no    是否在浏览器窗口显示菜单栏，默认为no
    resizable  yes/no    是否可以通过拖动浏览器窗口的边框改变大小，默认为no
    scrollbars yes/no    如果内容在页面中显示不下，是否允许滚动，默认为no
    status     yes/no    是否在浏览器窗口中显示状态栏，默认为no
    toolbar    yes/no    是否在浏览器窗口中显示工具栏，默认为no
    
    open('https://segmentfault.com/blog/xzavier','xzavier','width=600,height=400,top=200,left=400,resizable=yes');
    //类似一个弹窗弹出一个网页，open有返回值，返回window对象

### window方法-setTimeout和setInterval

JavaScript是单线程语言，它允许通过设置超时值和间歇时间值来调度代码在特定的时刻执行。setTimeout在指定的时间过后执行代码，setInterval是每隔指定的时间就执行一次代码。

#### setTimeout

接受两个参数：要执行的代码和超时时间（单位：毫秒）

    function timer() {
        console.log("xzavier's timer");
    }
    setTimeout(timer, 2000);

setTimeout()返回一个数值ID，表示这个超时调用。这个ID是超时调用的唯一标识符，可以通过它来取消超时调用。要取消尚未执行的超时调用，可以调用clearTimeout()方法并将相应的超时调用ID作为参数传递给它。

    function timer() {
        console.log("xzavier's timer");
    }
    var xzavier = setTimeout(timer, 2000);
    clearTimeout(xzavier);  //取消超时调用不执行

#### setInterval

setInterval按照指定的时间间隔重复执行代码，直至间歇调用被取消或者页面被卸载。设置间歇调用的方法是setInterval()，接受参数：要执行的代码和每次执行之前需要等待的毫秒数

    setInterval(function () {
        console.log('xzavier');
    }, 2000);

取消间歇调用方法和取消超时调用类似，使用clearInterval()方法。但取消间歇调用的重要性要远远高于取消超时调用，因为在不加干涉的情况下，间歇调用将会一直执行到页面关闭。

    var xzavier = setInterval(function () { 
        console.log('xzavier');
    }, 1000);
    clearInterval(xzavier);

一般来说，使用了setInterval就一定要使用clearInterval去清除，所以，使用超时调用来模拟间歇调用是一种最佳模式。因为，使用超时调用时，没必要跟踪超时调用ID，因为每次执行代码之后，如果不再设置另一次超时调用，调用就会自行停止。

### location对象

location是BOM对象之一，它提供了与当前窗口中加载的文档有关的信息。location对象是window对象的属性，也是document对象的属性。所以window.location和document.location等效。

#### location 对象的属性

    属性        描述的URL内容
    host        主机名：端口号
    hostname    主机名
    href        URL
    pathname    路径名
    port        端口号
    protocol    协议部分
    search      查询字符串
    hash        如果该部分存在，表示锚点部分

#### location 对象的方法

    方法           功能
    assign()  跳转到指定页面，与href等效
    reload()  重载当前URL
    repalce() 用新的URL 替换当前页面

F12打开调试器，console下打印window.location即可查看location的属性和方法。

    location.reload();        //最有效的重新加载，有可能从缓存加载
    location.reload(true);    //强制加载，从服务器源头重新加载
    location.assign('https://segmentfault.com/blog/xzavier');  //跳转到指定的URL
    location.replace('https://segmentfault.com/blog/xzavier'); //可以避免产生跳转前的历史记录，跳转后浏览器不能返回上一个页面

### navigator对象

navigator对象，其实也是window对象的属性，包含大量有关Web浏览器的信息，在检测浏览器及操作系统上非常有用。

    console.log(navigator.userAgent);  
    //用户代理头信息，很多时候用于判断浏览器  Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.116 Safari/537.36
    console.log(navigator.appCodeName);  
    //浏览器代码名 Mozilla 
    //appCodeName 属性是一个只读字符串，声明了浏览器的代码名。
    //在所有以 Netscape 代码为基础的浏览器中，它的值是 "Mozilla"。
    //为了兼容起见，在 Microsoft 的浏览器中，它的值也是 "Mozilla"。
    console.log(navigator.appName);  
    //官方浏览器名 Netscape
    //appName返回所使用浏览器的名称。该属性并不一定能返回正确的浏览器名称。
    //在基于 Gecko 的浏览器 （例如 Firefox）和基于 WebKit 的浏览器
    //（例如 Chrome 和 Safari）中，返回的浏览器名称都是 "Netscape". 
    console.log(navigator.appVersion);  
    //浏览器版本信息  5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.116 Safari/537.36
    console.log(navigator.cookieEnabled);  
    //是否启用cookie，返回true/false  
    console.log(navigator.javaEnabled);  
    //是否启用java，返回true/false  
    console.log(navigator.platform);  Win32
    //浏览器所在计算机平台  
    console.log(navigator.plugins);  
    //安装在浏览器中的插件数组  PluginArray {0: Plugin, 1: Plugin, 2: Plugin, 3: Plugin, 4: Plugin, length: 5}

### history对象

history对象是window 对象的属性，它保存着用户上网的记录，从窗口被打开的那一刻算起。
history对象有一个length属性，表示history 对象中的记录数。history对象有三个方法。

    方法                     功能
    back()       前往浏览器历史条目前一个URL，类似后退
    forward()    前往浏览器历史条目下一个URL，类似前进
    go(number)   跳转指定历史记录的URL

### screen对象

screen对象是window 对象的属性,用于获取屏幕的信息。

    属性             描述
    width        屏幕的宽度 
    height       屏幕的高度 
    availWidth   窗口可以使用的屏幕的宽度
    availHeight  窗口可以使用的屏幕的高度
    
    

document对象见： [温故js系列-DOM][1]


  [1]: https://segmentfault.com/a/1190000006623511