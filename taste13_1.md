## JavaScript-有意思的30题_题目

茶余饭后，来杯咖啡。题目与答案分来吧~下一篇答案；

### 1.以下表达式的运行结果是：

    ["1","2","3"].map(parseInt)
    A.["1","2","3"]
    B.[1,2,3]
    C.[0,1,2]
    D.其他 


### 2.以下表达式的运行结果是：

    [typeof null, null instanceof Object]
    A.["object",false]
    B.[null,false]
    C.["object",true]
    D.其他

### 3.以下表达式的运行结果是：

    [[3,2,1].reduce(Math.pow),[].reduce(Math.pow)]
    A.报错
    B.[9,0]
    C.[9,NaN]
    D.[9,undefined]

### 4.以下表达式的运行结果是：

    var val = 'value';
    console.info('Value id '+(val === 'value')?'Something':'Nothing');
    A.Something
    B.Nothing
    C.NaN
    D.其他

### 5.以下表达式的运行结果是：

    var name = 'World';
    (function(){
        if(typeof name === 'undefined'){
        var name = "Jack";
        console.info('Goodbye '+ name);
    }else{
        console.info('Hello ' + name);
    }
    })();
    A.Goodbye Jack
    B.Hello Jack
    C.Goodbye undefined
    D.Hello undefined

### 6.以下表达式的运行结果是：

    var START = END -100;
    var count = 0;
    for(var i = START ; i <= END ;i++){
        count ++;
    }
    console.info(count);
    A.0
    B.100
    C.101
    D.其他

### 7.以下表达式的运行结果是：

    var arr = [0,1,2];
    arr[10] = 10;
    arr.filter(function(x){return x === undefined});
    A.[undefined x 7]
    B.[0,1,2,10]
    C.[]
    D.[undefined]

### 8.以下表达式的运行结果是：

    var two = 0.2;
    var one = 0.1;
    var eight = 0.8;
    var six = 0.6;
    [two -one == one,eight- six == two];
    A.[true,true]
    B.[false,false]
    C.[true,false]
    D.其他
### 9.以下表达式的运行结果是：

    function showCase(value){
        switch(value){
            case 'A':
                console.info('Case A');
                break;
            case 'B':
                console.info('Case B');
                break;
            case undefined :
                console.info('undefined');
                break;
            default:
                console.info('Do not know!');
        }
    }
    showCase(new String('A'));
    A.Case A
    B.Case B
    C.Do not know
    D.undefined

### 10.以下表达式的运行结果是：

    function showCase(value){
        switch(value){
            case 'A':
                console.info('Case A');
                break;
            case 'B':
                console.info('Case B');
                break;
            case undefined :
                console.info('undefined');
                break;
            default:
            console.info('Do not know!');
        }
    }
    showCase(String('A'));
    A.Case A
    B.Case B
    C.Do not know
    D.undefined

### 11.以下表达式的运行结果是：

    function isOdd(num){
        return num % 2 == 1;
    }
    function isEven(num){
        return num % 2 == 0;
    }
    function isSane(num){
        return isEven(num)||isOdd(num);
    }
    var values = [7,4,'13',-9,Infinity];
    values.map(isSane);
    A.[true, true, true, true, true]
    B.[true, true, true, true, false]
    C.[true, true, true, false, false]
    D.[true, true, false, false, false]

### 12.以下表达式的运行结果是：

    [parseInt(3,8),parseInt(3,2),parseInt(3,0)]
    A.[3,3,3]
    B.[3,3,NaN]
    C.[3,NaN,NaN]
    D.其他

### 13.以下表达式的运行结果是：
　

    Array.isArray(Array.prototype)
    A.true
    B.false
    C.报错
    D.其他

### 14.以下表达式的运行结果是：

    var a = [0];
    if([0]){
        console.info(a == true);
    }else{
        console.info("false");
    }
    A.true
    B.false
    C."else"
    D.其他 

### 15.以下表达式的运行结果是：

    []==[]
    A.true
    B.false
    C.报错
    D.其他

### 16.以下表达式的运行结果是：

    [('5'+3),('5'-3)]
    A.["53",2]
    B.[8,2]
    C.报错
    D.其他

### 17.以下表达式的运行结果是：

    1+-+++-+1
    A.true
    B.false
    C.报错
    D.其他 

### 18.以下表达式的运行结果是：

    var arr = Array(3);
    arr[0] = 2
    arr.map(function(elem){return '1';});
    A.[2,1,1]
    B.["1","1","1"]
    C.[2,"1","1"]
    D.其他

### 19.以下表达式的运行结果是：

    function sidEffecting(arr){
        arr[0] = arr[2];
    }
    function bar(a,b,c){
        c = 10;
        sidEffecting(arguments);
        return a+b+c;
    }
    bar(1,1,1);
    A.3
    B.12
    C.报错
    D.其他

### 20.以下表达式的运行结果是：

    var a = 111111111111111110000;
    b = 1111;
    console.info(a+b);
    A.111111111111111111111
    B.111111111111111110000
    C.NaN
    D.Infinity 

### 21.以下表达式的运行结果是：

    var x = [].reverse;
    x();
    A.[]
    B.undefined
    C.报错
    D.window

### 22.以下表达式的运行结果是：

    Number.MIN_VALUE>0
    A.true
    B.false
    C.报错
    D.其他

### 23.以下表达式的运行结果是：

    [1<2<3,3<2<1]
    A.[true,true]
    B.[true,false]
    C.报错
    D.其他

### 24.以下表达式的运行结果是：

    2 == [[[2]]]
    A.true
    B.false
    C.undefined
    D.其他

### 25.以下表达式的运行结果是：

    [3.toString(),3..toString(),3...toString()]
    A.["3",error,error]
    B.["3","3.0",error]
    C.[error,"3",error]
    D.其他

### 26.以下表达式的运行结果是：

    (function(){
        var x1 =y1 =1;
    })();
    console.info(y1);
    console.info(x1);
    A.1，1
    B.error，error
    C.1，error
    D.其他

### 27.列举IE和FF脚本兼容性的问题

### 28.以下函数有什么问题？如何改进？

    function initButtons(){
        var body = document.body,button,i;
        for(i =0;i<5;i++){
            button = document.createElement("button");
            button.innerHTML = "Button" + i;
            button.addEventListener("click",function(e){
                alert(i);
            },false);
            body.appendChild(button);
        }
    }
    initButtons();

### 29.写一段代码

判断一个字符串中出现次数最多的字符，并统计出现的次数。

### 30.请问一下两段代码有什么不同？

    (1)setTimeout(function(){
        /*代码块*/
        setTimeout(arguments.callee,10);
    },10);
    (2)setInterval(function(){
        /*代码块*/
    },10);


声明：本文源于学习的时候保存的一份word文档。word复制自一篇博客“JavaScript30个你不可能全会的题目”，具体出处已无。