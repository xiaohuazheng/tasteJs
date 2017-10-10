## JavaScript-原型&原型链&原型继承

JavaScript的原型是一个重要的知识点，很多扩展应用都是从原型出发的。要说原型，我们先简单说一下函数创建过程。上一篇文章用闭包实现类和继承中用的是原型继承，今天就讲一讲原型继承。更多继承在后面的文章中更新。

### 函数创建过程

    function Xzavier() {};

    1.创建一个对象（有constructor属性及[[Prototype]]属性），其中[[Prototype]]属性不可访问、不可枚举。
    2.创建一个函数（有name、prototype属性），再通过prototype属性 引用第1步创建的对象。
    3.创建变量Xzavier，同时把函数的引用赋值给变量Xzavier。

#### 构造函数

构造函数是用来新建同时初始化一个新对象的函数，所以，任何一个函数都可以是构造函数。只是我们在写代码的时候一般首字母大写以便区分使用。

### 原型

每个函数在创建的时候js都自动添加了prototype属性，这就是函数的原型，原型就是函数的一个属性，类似一个指针。原型在函数的创建过程中由js编译器自动添加。

    function Xzavier() {
        this.name = 'xzavier';
        this.sex = 'boy';
        this.job = "jser";
    }
    //给A添加属性
    Xzavier.age = 23;
    //给A的原型对象添加属性
    Xzavier.prototype.sports = function() {console.log('basketball')}
    Xzavier.prototype = {
        hobbit1: function() {console.log('basketball');},
        hobbit2: function() {console.log('running');}
    }; 

### 原型链

在JavaScript中，每个对象都有一个[[Prototype]]属性，其保存着的地址就构成了对象的`原型链`。

[[Prototype]]属性是js编译器在对象被创建时自动添加的，其取值由new运算符的右侧参数决定。字面量的方式可转化为`new Obejct();`

    var x = new Xzavier();
    vae o = {};  //var o = new Obejct();

通过对象的[[Prototype]]保存对另一个对象的引用，通过这个引用往上进行属性的查找，这就是`原型链查找机制`。

对象在查找某个属性的时候，会首先遍历自身的属性，如果没有则会继续查找`[[Prototype]]`引用的对象，如果再没有则继续查找`[[Prototype]].[[Prototype]]`引用的对象，依次类推，直到`[[Prototype]].….[[Prototype]]`为`undefined`
    
    var str = new String('xzavier');
    str
    

![图片描述][1]
    
`Object.prototype`的`[[Prototype]]`就是`undefined`

    function Xzavier() {
        this.name = 'xzavier';
    }
    var x = new Xzavier();
    x.age = 23;
    
    console.log(x.job);  // 获取x的job属性 undefined

    1、遍历x对象本身，结果x对象本身没有job属性
    2、找到x的[[Prototype]]，也就是其对应的对象Xzavier.prototype，同时进行遍历。     Xzavier.prototype也没有job属性
    3、找到Xzavier.prototype对象的[[Prototype]]，指向其对应的对象Object.prototype。Object.prototype也没有job属性
    4、寻找Object.prototype的[[Prototype]]属性，返回undefined。

### 函数的变量和内部函数

说了函数的原型链，这里需要说一下的变量和内部函数。

#### 私有变量和内部函数

私有成员，即定义函数内部的变量或函数，外部无法访问。
    
    function Xzavier(){
        var name = "xzavier"; //私有变量
        var sports = function() {console.log('basketball')}; //私有函数 
    }
    var x = new Xzavier();
    x.name;  //undefined

如果要访问，需要对外提供接口。

    function Xzavier(){
        var name = "xzavier"; //私有变量
        var sports = function() {console.log('basketball')}; //私有函数
        return{
            name: name,
            sports: sports
        }
    }
    var x = new Xzavier();
    x.name;  //"xzavier"

#### 静态变量和内部函数

用点操作符定义的静态变量和内部函数就是实例不能访问到的变量和内部函数。只能通过自身去访问。

    function Xzavier(){
        Xzavier.name = "xzavier"; //静态变量
        Xzavier.sports = function() {console.log('basketball')}; //静态函数 
    }
    Xzavier.name; //"xzavier"
    var x = new Xzavier();
    x.name;  //undefined

#### 实例变量和内部函数

通过this定义给实例使用的属性和方法。

    function Xzavier(){
        this.name = "xzavier"; //实例变量
        this.sports = function() {console.log('basketball');}; //实例函数 
    }
    Xzavier.name; //undefined
    var x = new Xzavier();
    x.name;  //"xzavier"

### 原型链继承

有了原型链，就可以借助原型链实现继承。

    function Xzavier() {
        this.name = 'xzavier';
        this.sex = 'boy';
        this.job = "jser";
    }
    
    function X() {};

X的原型X.prototype原型本身就是一个Object对象。F12打开控制台输入函数，再打印`X.prototype`:

    Object {
        constructor: X()
        __proto__: Object
    }

prototype本身是一个Object对象的实例，所以其原型链指向的是Object的原型。

#### X.prototype = Xzavier.prototype
 
    X.prototype = Xzavier.prototype;

这样相当于把X的prototype指向了Xzavier的prototype；
这样只是继承了Xzavier的prototype方法，Xzavier中的自定义方法则不继承。

    X.prototype.love = "dog";

这样也会改变Xzavier的prototype，所以这样基础就不好。

#### X.prototype = new Xzavier()

    X.prototype = new Xzavier();

这样产生一个Xzavier的实例，同时赋值给X的原型，也即X.prototype相当于对象： 

    {
        name: "xzavier", 
        sex: "boy", 
        job: "jser",
        [[Prototype]] : Xzavier.prototype
    }

这样就把Xzavier的原型通过X.prototype.[[Prototype]]这个对象属性保存起来，构成了原型的链接。
不过，这样X产生的对象的构造函数发生了改变，因为在X中没有constructor属性，只能从原型链找到Xzavier.prototype，读出constructor:Xzavier。

    var x = new X;
    console.log(x.constructor);
    
    输出：
    Xzavier() {
        this.name = 'xzavier';
        this.sex = 'boy';
        this.job = "jser";
    }
 
手动改正：

    X.prototype.constructor = X;

这是X的原型就多了个属性constructor，指向X。这样就OK。

    function Xzavier() {
        this.name = 'xzavier';
        this.sex = 'boy';
        this.job = "jser";
    }

    function X(){}
    X.prototype = new Xzavier();
    var x = new X()
    x.name  // "xzavier"

### `[[Prototype]]，__proto__，prototype`

关于我们经常遇到的`[[Prototype]]，__proto__，prototype`：

我们在控制台打印 `var str = new String('xzavier')`，展开查看属性时，只会看到`__proto__`，所以起作用的是`__proto__`，`__proto__`是对象的内置属性，是每个对象都有的属性，但是这个属性使用不标准，所以不建议直接使用。但是，我们的原型链就是基于 `__proto__`的。通过构造函数得到的实例的 `__proto__` 属性，指向其对应的原型对象 `String.prototype`，这正如文中我们打印 `var str = new String('xzavier')` 中看到的一样。


`[[Prototype]]`是一个隐藏属性，指向的是这个对象的原型。几乎每个对象有一个`[[prototype]]`属性。

而`prototype`是每个函数对象都具有的属性，指向原型对象，如果原型对象被添加属性和方法，那么由应的构造函数创建的实例会继承`prototype`上的属性和方法，这也是我们在代码中经常遇到的。构造函数产生实例时，实例通过其对应原型对象的 constructor 访问对应的构造函数对象。所以，我们继承出来的实例往往没有constructor，只是通过原型链查找，会让我们产生错觉，可参见本系列原型链文章。

### hasOwnProperty

hasOwnProperty是Object.prototype的一个方法，判断一个对象是否包含自定义属性而不是原型链上的属性。
hasOwnProperty 是JavaScript中唯一一个处理属性但是不查找原型链的函数。

    function Xzavier() {
        this.name = 'xzavier';
        this.sex = 'boy';
        this.job = "jser";
    }
    //给A的原型对象添加属性
    Xzavier.prototype.sports = function() {console.log('basketball');};
    
    var x = new Xzavier();
    x.name; // 'xzavier'
    'sex' in x; // true
    
    x.hasOwnProperty('job'); // true
    x.hasOwnProperty('sports'); // false

当检查对象上某个属性是否存在时，hasOwnProperty 是非常推荐的方法。


继承在js中使用频繁。ES6也设计了专门的CLASS语法糖供开发者使用。
更多继承方法在新的文章中更新...

难得周末，应该运动O(∩_∩)O~ 打打篮球，运动运动，有代码，有篮球，有生活。。。
长时间不动肩膀（其实身体各地方都是），还真疼啊。希望程序猿们都健健康康的！！！



  [1]: https://sfault-image.b0.upaiyun.com/149/829/1498299667-584cb6c104830_articlex