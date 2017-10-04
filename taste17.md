## Object对象

在 JavaScript 中，对象，是对一些具体事物的一种抽象，所有其他对象都继承自这个对象。Object 是一个无序集合，将变量和值集合在一起，可以存放任意类型对象（JavaScript中的一切皆对象，这句话应该没有对错，无论正反两个方面，支持者都能说出他们的解释）。

### 对象创建

#### 字面量方式

    var obj = {
        key: 'value',
        name : 'xzavier',
        sex : 'male',
    };

#### 构造函数方式

    var obj = new Object();
    obj.key = 'value';
    obj.name = 'xzavier';
    obj.sex = 'male';

也可以这样（不过这样的话，为何不选择字面量方式）：
    
    var obj = new Object({
        key: 'value',
        name : 'xzavier',
        sex : 'male',
    });


字面量方式和new方式的写法是等价的，返回的结果是同种类的对象。

字面量方式比较好的是你可以在声明的时候一次性添多个属性和值，即键/值对；
而构造函数方式，你必须在构造完成之后一个一个地添加属性。

所以，你经常都会被建议使用字面量的方式，我们也实践得非常不错。很多时候，我们只有在场景需要的情况下才会使用new 构造函数的方式。比如说抛出错误、创建含有变量的正则等：
    
    new Error(..)
    new RegExp('xzav' + i + 'er');

所以，大多情况下我们都使用字面量方式，一般不需要明确地创建对象。JavaScript会在必要的时候自动地将一些基本类型转换为对象类型，以便你能使用对象类型中的属性：

    var x = 'xx';
    x.length; // 2  拥有String类型对象的属性
    var z = '123.321';
    z.toFixed(2); // "123.32" 拥有Number类型对象的方法

### 内置对象

JavaScript中，有很多内置的Object的子类型，我们通常称为内置对象，它们实际上是内建的函数，每一个都可以作为构造函数 `new` 出一个新构造的相应子类型的实例，当然，也是对象。

    String
    Number
    Boolean
    Math
    Function
    Array
    Date
    RegExp
    Error

如上面所说，创建这些子类型也基本使用字面量的方法。检测类型的话使用

    Object.prototype.toString.call(obj);

检测的最全。具体参考文章： [数据类型检测][1]

### Object.prototype对象

在JavaScript中，几乎所有对象都是Object的实例。而Object有一个属性prototype，指向原型对象（js里所有构造函数都有一个prototype属性，指向原型对象）。我们在实例化一个对象时，实例会继承原型对象上的属性和方法。

可以控制台查看 `String.prototype`
然后再： 

    var str = new String('xzavier');

我们的str继承了String.prototype上的属性和方法，String又继承了Obeject.prototype上的方法。

不过说是继承，说是指向引用比较好。因为对象在查找某个属性的时候，会首先遍历自身的属性，如果没有则会继续查找`[[Prototype]]`引用的对象，如果再没有则继续查找`[[Prototype]].[[Prototype]]`引用的对象，依次类推，直到`[[Prototype]].….[[Prototype]]`为`undefined`

`Object.prototype`的`[[Prototype]]`就是`undefined`

![图片描述][2]

在控制台打印仔细去看：
    
    String.prototype  // ... ...
    Obeject.prototype  // ...
    Obeject.prototype.prototype // undefined

你还可以在控制台输入：
    
    var str = new String('xzavier');
    str

然后一层层的查看`__proto__`，你自己创建的构造函数同此。其实我们的实例没有继承到方法和属性，只是添加了个原型属性，使用的时候往原型链上查找，可以找到父级以及更上一层的原型链上的方法，然后使用。

具体参见： [原型链][3]

回过头来，修改Object.prototype就会影响到它的子孙后代。改变Object原型对象会修改到所有通过原型链继承的对象，除非实例的相关属性和方法沿原型链进一步覆盖。这是一个非常强大的，存在潜在危险的机制，可以覆盖或扩展对象的行为。
    
    Object.prototype.xxx = function(){ console.log('xxx')};
    
    var str = new String('xzavier');
    str.xxx();  // xxx

所以：
    
    Don’t modify objects you don’t own

不过，很多时候我们自己写代码，用原型属性扩展方法是非常实用的。
    
    // 数组去重
    Array.prototype.unique = function(){
        return [...new Set(this)];
    }
    [1,2,3,'4',3,4,3,1,'34',2].unique(); //[1, 2, 3, "4", 4, "34"]

### 对象的遍历

数组是对象，字符串是对象，都可以遍历。遍历详解参考： [流程控制][4]

这儿就说说对象{}类型的遍历：

`for...in` 语句可以用来枚举对象的属性，为遍历对象属性设计的。
    
    var xzavier = { 
        'name' : 'xzavier', 
        'age' : 23,
        'job' : 'Jser',
        'width' : 100,
        'height' : 100,
        'border' : 10
    };
    for (var i in xzavier) { 
        console.log(i);
    }
    //name age job width height border
    for (var i in xzavier) { 
        console.log(xzavier[i]);
    }
    //xzavier 23 Jser 100 100 10
    

在设计对象的时候，键值最好是以字符串，而非数字字符串，如'0', '123',这样会导致对象重排。
比如：

    var obj = {
        '0': 1,
        'abc': 2,
        'def': 3,
        '2': 4,
        'ghi': 5
    };
    

打印obj:

    Object {0: 1, 2: 4, abc: 2, def: 3, ghi: 5, __proto__: Object }
    

用for in 语句输出：

    for (var i in obj) {
      console.log(oo[i]);
    }  
    // 1 4 2 3 5 
    
    for (var i in obj) {
      console.log(i);
    }
    // 0 2 abc def ghi
    

虽然我们平时使用不会受到什么影响，也不会这么设计。但是你在不在乎，它始终在这里。

react的for循环key值就建议不要以index为值，也有这个原因，具体可学习react。

### 对象的判断

我们经常会遇到判断对象类型，这儿就不说typeof了，详情请参考：[数据类型判断][5]

这儿简写一下这个不会出错的方法：

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

但是，很多时候我们在处理数据的时候，需要判断一个对象是否为{}：

    var isEmptyObject = function(obj) {
        for (var name in obj) {
            return false;
        }
        return true;
    };
    isEmptyObject({});  // true
    isEmptyObject({name: 'xzavier'}); //false

### 对象属性值访问

阅读了[温故js系列（1）- 数据类型存储][6]，我们知道，引用数据类型值指保存在堆内存中的对象。也就是，变量中保存的实际上的只是一个指针，这个指针指向内存中的另一个位置，该位置保存着对象，访问方式是按引用访问。

    var obj = {
        key: 'value',
        name : 'xzavier',
        sex : 'male',
        'x-v': 'xz',
        'xx!': 'xxxx',
    }; 

在访问一个对象的一个属性时，使用`.`或`["..."]`操作符。`obj.key` 和 `obj['key']` 都访问obj中相同的位置，返回值都是`'xzavier'`，所以这两种方式都在代码中经常使用到。

而这两种访问方式的区别是，`.`操作符后面需要跟一个标识符（Identifier）兼容的属性名，而["..."]语法基本可以接收任何兼容UTF-8/unicode的字符串作为属性名。

    obj.x-v  // 报错
    obj.xx!  // 报错
    obj['x-v']  // "xz"
    obj['xx!']  // "xxxx"

因为`x-z`，`xx!`不是一个合法的标识符属性名。

`["..."]` 还可以动态组建属性名，这在合适的场景下非常好用。

    var obj = {
        number1: 'xx',
        number2: 'yy',
        number3: 'zz'
    }
    var number = 2;
    var select_name = obj['number' + number];  // 'yy'

### 属性描述符

    var xz = {name: 'xzavier'}
    Object.getOwnPropertyDescriptor( xz, "name" );

正如函数名，获取属性描述符，打印如下：

    //  configurable: true,  是否可配置
    //  writable: true,   是否可写
    //  value: 'xzavier',
    //  enumerable: true   是否可枚举
    //  __proto__: Object

我们很少关注到这些，若有需要，我们关注的莫过于`configurable，writable，enumerable`等。
可以通过Object的defineProperty方法修改值的属性，一般我们使用defineProperty是给对象添加属性，在这个使用的基础上，可以对这个值的属性进行配置。

    Object.defineProperty( obj, "name", {
        value: 'xzavier-1',
        configurable: true,  //是否可配置
        writable: false,   //是否可写
        enumerable: true   //是否可枚举
    });
    
    obj.name // xzavier-1
    obj.name = 'xzavier-2' // 不会报错，但在"use strict"模式下，会报错TypeError
    obj.name // xzavier-1  值没有被修改

这是`writable`，接下来说说`enumerable`，是否可枚举：

`enumerable`表述属性是否能在特定的对象属性枚举操作中出现，比如在for..in，for...of中遍历。如果enumerable被设置为false，那么这个属性将不会出现在枚举中，不过它依然可以被属性访问方式访问。

    var obj = {
        number1: 'xx',
        number2: 'yy',
        number3: 'zz'
    }
    Object.defineProperty( obj, "name0", {
        value: 'xzavier-1',
        configurable: true,  //是否可配置
        writable: true,   //是否可写
        enumerable: false   //是否可枚举
    });
    for(var i in obj) {
        console.log(i);  // number1 number2 number3  不会出现number0
    }
    for(var i of keys(obj)) {
        console.log(i);  // number1 number2 number3  不会出现number0
    }
    

再把enumerable设置为true就可以继续在枚举中遍历到了。

    Object.defineProperty( obj, "name0", {
        enumerable: true   //是否可枚举
    });
    for(var i in obj) {
        console.log(i);  // number1 number2 number3 number0
    }
    for(var i in keys(obj)) {
        console.log(i);  // number1 number2 number3 number0
    }

最后说下`configurable` ，表示属性是否可以进行以上操作，即是否可配置。它是一个单向操作，不可逆。

    Object.defineProperty( obj, "name4", {
        value: 'xzavier-4',
        configurable: false,  //是否可配置
        writable: false,   //是否可写
        enumerable: true   //是否可枚举
    }); 

属性的configurable一旦被设为false，将不可逆转，再用defineProperty设置属性将会报错。

    Object.defineProperty( obj, "name4", {
        value: 'xzavier-4',
        configurable: true,  //是否可配置
        writable: true,   //是否可写
        enumerable: true   //是否可枚举
    });
    // TypeError

### Object方法

#### `Object.assign()`

`Object.assign(target, ...sources)`方法用于从一个或多个源对象的所有可枚举属性的值复制到目标对象上，有相同属性的时候，目标对象上的属性会被覆盖，最终返回目标对象。
接收参数：`target目标对象`，`...sources一到多个源对象` 不传则直接返回目标对象

我们可以：

1.复制对象

    var obj = { a: 1 };
    var obj_1 = Object.assign({}, obj);  // { a: 1 }

2.合并对象

    var o1 = { a: 1 };
    var o2 = { b: 2 };
    var o3 = { c: 3 };

    var obj = Object.assign({}, o1, o2, o3);  // { a: 1, b: 2, c: 3 }

#### `Object.create()`

`Object.create(prototype, descriptors)` 方法创建一个具有指定原型且可选择性地包含指定属性的对象。接收参数：`prototype`，必需，要用作原型的对象，可以为 null。`descriptors`，可选，包含一个或多个属性描述符的 JavaScript 对象。数据属性描述符包含`value`，以及 `writable，enumerable，configurable`，即上面所讲。

我们可以：

1.创建一个新对象：

    var obj = {
        name: 'xzavier'
    }
    
    var o = Object.create(obj);
    o.name; // 'xzavier'
    
    // 但是 name属性并非o的自定义属性
    o.hasOwnProperty('name'); //false  你在浏览器操作之后展开也可以清晰的看到

2.创建一个空对象（没有原型的对象）

`Object.create(null)`创建一个拥有空`[[Prototype]]`链接的对象，即这个对象没有原形链。

    var obj = Object.create(null);

在控制台会看到返回 object[ No Properties ]
那这样的东西创建来又什么用呢，它可以作为一种数据存储方式，不用担心它会被原型之类的污染，不用担心会有原型链查找。它就是一个不会存在一个你意想不到的属性的存储结构。所以，可以放心使用它。

3.继承

    function Parent() {}
        
    Parent.prototype.say = function() {
        console.info("Hello World");
    };
    
    function Childs() {
      Parent.call(this);
    }
    
    Childs.prototype = Object.create(Parent.prototype);
    
    var child = new Childs();

    child instanceof Childs; //true.
    child instanceof Parent; //true.
    
    child.say();  // Hello World

#### `Object.is()`

ES6之前，比较两个值是否相等，使用相等运算符（==）和严格相等运算符（===）。具体参见：[代码中的哪些判断][7]。

而`Object.is()`的出现主要是让以下情况出现：

    +0 === -0 //true
    NaN === NaN // false
    
    Object.is(+0, -0) // false
    Object.is(NaN, NaN) // true

`Object.is()`使用的是严格相等运算符（===）做判断，对`+0，-0，NaN`做了特殊处理

#### `Object()`

    Object() // {}
    Object({})  // {}
    Object(undefined) // {}
    Object(null) // {}  
    Object(1) // 同 new Number(1)
    Object(NaN) // 同 new Number(NaN)
    Object('xzavier') // 同 new String('xzavier')
    Object(false) // 同 new Boolean(false) 
    Object([1,2,3]) // [1,2,3]
    Object({name: 'xzavier'}) // {name: "xzavier"}
    Object(function x(){}) // function x(){}

`Object()`创造一个“真”对象，返回的都是一个`truthy`值，是一个对象，所以在`if()`判断中都是一个真值。

#### `Object.prototype`上的方法

`Object.prototype`上的方法往往都会被继承到你实例化的或者字面量形式声明的数据类型中。我们可以直接在实例上使用：

    [1,2,3].toString();  //"1,2,3"
    ({a: 1}).valueOf();  // {a: 1}
    ......
    

### 对象的其他使用

关于对象的使用，上面所罗列的都是我们经常遇到的。我们也经常使用对象的特性，做一些事情。

数组去重：

    Array.prototype.unique = function() {
          var arr = [];
          var hash = {};
          for (var i = 0; i < this.length; i++) {
            var item = this[i];
            var key = typeof(item) + item
            if (hash[key] !== 1) {
                  arr.push(item);
                  hash[key] = 1;
            }
          } 
          return arr;
    }
    [1,2,3,'4',3,4,3,1,'34',2].unique(); //[1, 2, 3, "4", 4, "34"]

hash去重的核心是构建了一个 hash 对象来替代 indexOf。判断hash的key是否已经存在来去重。

最后说一下，文章里面提到的`[[Prototype]]，__proto__，prototype`。

你打印来看，我们只会看到`__proto__`，所以起作用的是`__proto__`，`__proto__`是对象的内置属性，是每个对象都有的属性，但是这个属性使用不标准，所以不建议直接使用。但是，我们的原型链就是基于 `__proto__`的。通过构造函数得到的实例的 `__proto__` 属性，指向其对应的原型对象 `String.prototype`，这正如文中我们打印 `var str = new String('xzavier')` 中看到的一样。


`[[Prototype]]`是一个隐藏属性，指向的是这个对象的原型。几乎每个对象有一个`[[prototype]]`属性。

而`prototype`是每个函数对象都具有的属性，指向原型对象，如果原型对象被添加属性和方法，那么由应的构造函数创建的实例会继承`prototype`上的属性和方法，这也是我们在代码中经常遇到的。构造函数产生实例时，实例通过其对应原型对象的 constructor 访问对应的构造函数对象。所以，我们继承出来的实例往往没有constructor，只是通过原型链查找，会让我们产生错觉，可参见本系列原型链文章。

  [1]: https://segmentfault.com/a/1190000005863067#articleHeader4
  [2]: https://sfault-image.b0.upaiyun.com/149/829/1498299667-584cb6c104830_articlex
  [3]: https://segmentfault.com/a/1190000006876041
  [4]: https://segmentfault.com/a/1190000006669505
  [5]: https://segmentfault.com/a/1190000005863067
  [6]: https://segmentfault.com/a/1190000005863067
  [7]: https://segmentfault.com/a/1190000006672446