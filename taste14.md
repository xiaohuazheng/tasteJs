## JavaScript-闭包

闭包(closure)是一个让人又爱又恨的something，它可以实现很多高级功能和应用，同时在理解和应用上有很多难点和需要小心注意的地方。

### 闭包的定义

闭包，官方对闭包的解释是：一个拥有许多变量和绑定了这些变量的环境的表达式（通常是一个函数），因而这些变量也是该表达式的一部分。

简单来说，闭包就是能够读取其他函数内部变量的函数。在Javascript中，只有函数内部的子函数才能读取函数的局部变量，所以，可以把闭包理解成：定义在一个函数内部的函数，也就是函数嵌套函数，给函数内部和函数外部搭建起一座桥梁。

### 闭包的特点

    1. 定义在一个函数内部的函数。
    2. 函数内部可以引用函数外部的参数和变量。
    3. 作为一个函数变量的一个引用，当函数返回时，其处于激活状态。
    4. 当一个函数返回时，一个闭包就是一个没有释放资源的栈区。函数的参数和变量不会被垃圾回收机制回收。

### 闭包的形成

Javascript允许使用内部函数，可以将函数定义和函数表达式放在另一个函数的函数体内。而且，内部函数可以访问它所在的外部函数声明的局部变量、参数以及声明的其他内部函数。当其中一个这样的内部函数在包含它们的外部函数之外被调用时，就会形成闭包。

    function a() {  
        var i = 0;  
        function b() { 
            console.log(i++); 
        }  
        return b; 
    }
    var c = a(); 
    c();

### 闭包的缺点

1.由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大。所以在闭包不用之后，将不使用的局部变量删除，使其被回收。在IE中可能导致内存泄露，即无法回收驻留在内存中的元素，这时候需要手动释放。

    function a() {  
        var i = 1;  
        function b() { 
            console.log(i++); 
        }  
        return b; 
    }
    var c = a(); 
    c(); //1
    c(); //2
    c(); //3   i不被回收
    c = null;  //i被回收

2.闭包会在父函数外部，改变父函数内部变量的值。如果你把父函数当作对象使用，把闭包当作它的公用方法，把内部变量当作它的私有属性，要小心，不要随便改变父函数内部变量的值。

    var Xzavier = { 
        ten:10,  
        addTen: function(num) {  
           return this.ten + num;   //给一个数加10 
       }    
    }
     
    console.log(Xzavier.addTen(15));  //25
    Xzavier.ten = 20; 
    console.log(Xzavier.addTen(15));  //35

### 内存泄露

内存泄漏指一块被分配的内存既不能使用，又不能回收，直到浏览器进程结束。

出现原因：

    1.循环引用：含有DOM对象的循环引用将导致大部分当前主流浏览器内存泄露。循环 引用，简单来说假如a引用了b,b又引用了a,a和b就构成了循环引用。
    2.JS闭包：闭包，函数返回了内部函数还可以继续访问外部方法中定义的私有变量。
    3.Dom泄露，当原有的DOM被移除时，子结点引用没有被移除则无法回收。  

  
### JavaScript垃圾回收机制

Javascript中，如果一个对象不再被引用，那么这个对象就会被GC(garbage collection)回收。如果两个对象互相引用，而不再被第3者所引用，那么这两个互相引用的对象也会被回收。垃圾回收不是时时的，因为其开销比较大，所以垃圾回收器会按照固定的时间间隔周期性的执行。

函数a被b引用，b又被a外的c引用，这就是为什么函数a执行后不会被回收的原因。

垃圾回收的两个方法：

标记清除法：

    1.垃圾回收机制给存储在内存中的所有变量加上标记，然后去掉环境中的变量以及被环境中变量所引用的变量（闭包）。
    2.操作1之后内存中仍存在标记的变量就是要删除的变量，垃圾回收机制将这些带有标记的变量回收。

引用计数法：

    1.垃圾回收机制给一个变量一个引用次数，当声明了一个变量并将一个引用类型赋值给该变量的时候这个值的引用次数就加1。
    2.当该变量的值变成了另外一个值，则这个值得引用次数减1。
    3.当这个值的引用次数变为0的时候，说明没有变量在使用，垃圾回收机制会在运行的时候清理掉引用次数为0的值占用的空间。

### 闭包的应用

#### 1.维护函数内的变量安全,避免全局变量的污染。

函数a中i只有函数b才能访问，而无法通过其他途径访问到。

    function xzavier(){
        var i = 1;
        i++;
        console.log(i);
    }
    xzavier();   //2 
    console.log(x);   // x is not defined                 
    xzavier();   //2

#### 2.维持一个变量不被回收。

由于闭包，函数a中i的一直存在于内存中，因此每次执行c()，都会给i自加1，且i不被垃圾回收机制回收。
    function a() {  
        var i = 1;  
        function b() { 
            console.log(i++); 
        }  
        return b; 
    }
    var c = a(); 
    c();  //1
    c();  //2
    c();  //3

#### 3.通过第1点的特性设计私有的方法和属性。

    var xzavier = (function(){
        var i = 1;
        var s = 'xzavier';
        function f(){
            i++;
            console.log(i);
        }
        return {
            i:i,
            s:s,             
            f:f
        }
    })();
    xzavier.s;     //'xzavier'
    xzavier.s;     //1
    xzavier.f()    //2

#### 4.操作DOM获取目标元素

方法2即使用了闭包的方法，当然操作DOM还是有别的方法的，比如事件委托就比较好用。

    ul id="test">
        <li>first</li>
        <li>second</li>
        <li>third</li>
    </ul>
    // 方法一：this方法
    var lis = document.getElementById('test').getElementsByTagName('li');
    for(var i = 0;i < 3;i++){
        lis[i].index = i;
        lis[i].onclick = function(){
            console.log(this.index);
        };
    } 
    // 方法二：闭包方法
    var lis = document.getElementById('test').getElementsByTagName('li');
    for(var i = 0;i < 3;i++){
        lis[i].index = i;
        lis[i].onclick = (function(val){
            return function() {
                console.log(val);
            }
        })(i);
    }
    // 方法三 事件委托方法
    var oUl = document.getElementById('test');
    oUl.addEventListener('click',function(e){
        var lis = e.target;
        console.log(lis); 
    });

#### 5.封装模块

逻辑随业务复杂而复杂O(∩_∩)O~

    var Xzavier = function(){       
        var name = "xzavier";       
        return {    
           getName : function(){    
               return name;    
           },    
           setName : function(newName){    
               name = newName;    
           }    
        }    
    }();    
    
    console.log(person.name); //undefined，变量作用域为函数内部，外部无法访问    
    console.log(person.getName()); // "xzavier" 
    person.setName("xz");    
    console.log(person.getName());  //"xz"

#### 6.实现类和继承

    function Xzavier(){       
        var name = "xzavier";       
        return {    
           getName : function(){    
               return name;    
           },    
           setName : function(newName){    
               name = newName;    
           }    
        }    
    }
    
    var xz = new Xzavier();  //Xzavier就是一个类，可以实例化
    console.log(xz.getName());  // "xzavier"  

这里是原型继承，我会在下一篇文章讲一讲原型继承。

    var X = function(){};
    X.prototype = new Xzavier(); 
    X.prototype.sports = function(){
        console.log("basketball");
    };
    var x = new X();
    x.setName("xz");
    x.sports();  //"basketball"
    console.log(x.getName());  //"xz"

### JavaScript作用域链
JavaScript作用域

    作用域就是变量与函数的可访问范围，即作用域控制着变量与函数的可见性和生命周期。
    在JavaScript中，变量的作用域有全局作用域和局部作用域两种。

JavaScript作用域链

    JavaScript函数对象拥有可以通过代码访问的属性和一系列仅供JavaScript引擎访问的内部属性。
    其中一个内部属性是[[Scope]]，该内部属性包含了函数被创建的作用域中对象的集合。
    这个集合被称为函数的作用域链。

执行上下文

    当函数执行时，会创建一个执行上下文(execution context)，执行上下文是一个内部对象，定义了函数执行时的环境。
    每个执行上下文都有自己的作用域链，用于标识符解析。
    当执行上下文被创建时，而它的作用域链初始化为当前运行函数的[[Scope]]包含的对象。

活动对象

    这些值按照它们出现在函数中的顺序被复制到执行上下文的作用域链中。
    它们共同组成了一个新的对象，活动对象(activation object)。
    该对象包含了函数的所有局部变量、命名参数、参数集合以及this，然后此对象会被推入作用域链的前端。
    当执行上下文被销毁，活动对象也随之销毁。
    活动对象是一个拥有属性的对象，但它不具有原型而且不能通过JavaScript代码直接访问。

查找机制:

    1.当函数访问一个变量时，先搜索自身的活动对象，如果存在则返回，如果不存在将继续搜索函数父函数的活动对象，依次查找，直到找到为止。
    2.如果函数存在prototype原型对象，则在查找完自身的活动对象后先查找自身的原型对象，再继续查找。
    3.如果整个作用域链上都无法找到，则返回undefined。

在执行上下文的作用域链中，标识符所在的位置越深，读写速度就会越慢。全局变量总是存在于执行上下文作用域链的最末端，因此在标识符解析的时候，查找全局变量是最慢的。

so

    在编写代码的时候应尽量少使用全局变量，尽可能使用局部变量。
    我们经常使用局部变量先保存一个多次使用的需要跨作用取的值再使用。

### 再析闭包

    function a() {  
        var i = 1;  
        function b() { 
            console.log(i++); 
        }  
        return b; 
    }
    var c = a(); 
    c();
    
    1.当定义函数a，js解释器将函数a的作用域链设置为定义a时a所在的环境。
    2.执行函数a的时候，a会进入相应的执行上下文。
    3.在创建执行上下文的过程中，首先会为a添加一个scope属性，即a的作用域，其值就为a的作用域链。
    4.然后执行上下文会创建一个活动对象。
    5.创建完活动对象后，把活动对象添加到a的作用域链的最顶端。此时a的作用域链包含了两个对象：a的活动对象和window对象。
    6.接着在活动对象上添加一个arguments属性，它保存着调用函数a时所传递的参数。
    7.最后把所有函数a的形参和内部的函数b的引用也添加到a的活动对象上。    
      在这一步中，完成了函数b的的定义（如同a），函数b的作用域链被设置为b所被定义的环境，即a的作用域。
    8.整个函数a从定义到执行的步骤完成。

a返回函数b的引用给c，因为函数b的作用域链包含了对函数a的活动对象的引用，也就是说b可以访问到a中定义的所有变量和函数。函数b被c引用，函数b又依赖函数a，因此函数a在返回后不会被GC回收，所以形成了闭包。


