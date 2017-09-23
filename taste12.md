## JavaScript-Ajax&&node后端

2005年Jesse James Garrett 发表了一篇文章，标题为：“Ajax：A new Approach to Web Applications”。他在这篇文章里介绍了一种技术叫：Ajax，即Asynchronous JavaScript And XML。这种技术能够向服务器请求数据而不须刷新整个页面，会带来更好的用户体验。

### XMLHttpRequest

Ajax技术核心是XMLHttpRequest 对象(简称XHR)，提供了向服务器发送请求和解析服务器响应提供了流畅的接口。能够以异步方式从服务器获取更多的信息，在不刷新网页的情况下，更新服务器最新的数据到页面上。IE7+、Firefox、Opera、Chrome 和Safari 都支持原生的XHR对象。

    var xhr = new XMLHttpRequest();  //实例化XMLHttpRequest

虽然说现在基本不用去兼容IE6了，有句话叫啥：你不用为了一点用户去提高开发成本。不过，学习的时候还是要摸清楚。IE6 及以下，那么我们必须还需要使用ActiveX 对象通过MSXML 库来实现。兼容一下：

    function CreateXHR() {
        if(window.XMLHttpRequest){
            return new XMLHttpRequest();
        }else{
            return new ActiveXObject('Microsoft.XMLHTTP');
        }
    }
    var xhr = new CreateXHR();  

### Ajax实现

#### 1. 实例化XMLHttpRequest

    var xhr = new CreateXHR();

#### 2. 连接服务器

在使用XHR 对象时，必须先调用open()方法，它接受三个参数：要发送的请求类型(get、post)、请求的URL 和表示是否异步，true 为异步，false 为同步。

    xhr.open('get', 'xzavier', true); 

open()方法并不会真正发送请求，而只是启动一个请求以备发送。在send()之前，有一个设置自定义请求头部的方法setRequestHeader('key', 'value');放在open 方法之后，send 方法之前。不过，一般在post提交表单时用到：

    xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');

#### 3. 发送请求

当open()方法准备好之后，通过send()方法进行发送请求，send()方法接受一个参数，作为请求主体发送的数据。如果不需要则，必须填null。

    xhr.send(null); 

执行send()方法之后，请求就会发送到服务器上。

#### 4. 接收响应

当请求发送到服务器端，收到响应后，响应的数据会自动填充XHR 对象的属性。一共有四个属性，常用前面三个：

        属性                       说明
    responseText    作为响应主体被返回的文本
    status          响应的HTTP 状态
    statusText      HTTP 状态的说明
    responseXML     如果响应主体内容类型是"text/xml"或"application/xml"，则返回包含响应数据的XML DOM 文档

接受响应之后，第一步检查status 属性，以确定响应已经成功返回。一般而已HTTP状态代码为200作为成功的标志。HTTP状态代码：

    HTTP      状态码                  说明
    200   OK                    服务器成功返回
    400   Bad Request           语法错误导致服务器无法识别
    404   Not found             URL不存在
    500   Internal Server Error 服务器遇到意外错误无法完成请求
    503   ServiceUnavailable    服务器过载导致无法完成请求

列几个比较常用的状态码，详见：[状态码][1]
同步方式：

    var oButton = document.getElementById('myButton');
    oButton.onclick = function() {
        var xhr = new createXHR();
        xhr.open('get', 'xzavier', false); //false同步
        xhr.send(null);
        if (xhr.status == 200) { 
            console.log(xhr.responseText); 
        } else {
            console.log('error status:' + xhr.status + 'info:' + xhr.statusText);
        }
    }

同步只是这项技术的一种使用方式，但是很少用，使用异步调用才是常用的。使用异步调用的时候，需要触发readystatechange事件，然后检测readyState属性，属性值：

    属性值    状态         说明
      0     未初始化    未调用open()方法
      1     启动       已经调用open()方法，未调用send()方法
      2     发送       已经调用send()方法，未接受响应
      3     接受       已经接受到部分响应数据
      4     完成       已经接受到全部响应数据
  
  异步方式：

    var oButton = document.getElementById('myButton');
    oButton.onclick = function() {
        var xhr = new createXHR();
        xhr.onreadystatechange = function () {
            if (xhr.readyState == 4) {
                if (xhr.status == 200) {
                    alert(xhr.responseText);
                } else {
                    console.log('error status:' + xhr.status + 'info:' + xhr.statusText);
                }
            }
        }
        xhr.open('get', '/xzavier', true); //true同步
        xhr.send(null);
    }

整个ajax异步可以总结为：
1. 创建XMLHttpRequest对象,即创建一个异步调用对象
2. 创建一个新的HTTP请求,并指定该HTTP请求的方法、URL及是否异步
3. 设置响应HTTP请求状态变化的函数
4. 发送HTTP请求
5. 获取异步调用返回的数据
6. 使用JavaScript和DOM实现局部刷新

### GET与 POST

在提供服务器请求的过程中，有两种方式，分别是：GET和POST。

GET： 一般用于信息获取，用URL传递参数，对发送信息数量有限制，一般2000个字符

POST：一般用于修改服务器上的资源，对所发送的信息没有限制

GET： 是通过地址栏来传值

POST：是通过提交表单来传值

在以下情况中，请使用 POST 请求：

1. 无法使用缓存文件（更新服务器上的文件或数据库）
2. 向服务器发送大量数据（POST 没有数据量限制）
3. 发送包含未知字符的用户输入时，POST比 GET更稳定也更可靠

#### GET

GET请求是最常见的请求类型，常用于向服务器查询某些信息。必要时，可以将查询字符串参数追加到URL的末尾，以便提交给服务器。

    xhr.open('get', 'xzavier?name=' + name + '&sex='+ sex , true);

通过URL 后的问号给服务器传递键值对数据，服务器接收到返回响应数据。特殊字符传参产生的问题可以使用encodeURIComponent()进行编码处理，中文字符的返回及传参，可以讲页面保存和设置为utf-8 格式即可。

    xhr.open('get', 'xzavier?name=' + encodeURIComponent(name) + '&sex='+ encodeURIComponent(sex) , true);

#### POST

POST 请求可以包含非常多的数据，我们在使用表单提交的时候，很多就是使用的POST传输方式。
发送POST请求的数据，不会跟在URL后面，而是通过send()方法向服务器提交数据。向服务器发送POST请求由于解析机制的原因，需要进行请求头部的处理。

    xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');

### Ajax封装

jquery的ajax方法非常好用，不用写很多代码去区别get还是post，异步还是同步。当然了，自己用的话jquery已经很完美了，用着比自己封装的好用多了，当然，全球互联网有大部分都用着jquery插件。这儿就不说jquery的ajax，我们自己来封装一个，封装一个东西也是对基础知识的巩固和提升。

    //名值对转换为字符串
    function params(data) {
        var arr = [];
        for (var i in data) {
            arr.push(encodeURIComponent(i) + '=' + encodeURIComponent(data[i]));
        }
        return arr.join('&');
    }
    function ajax(obj) {
        var xhr = createXHR();
        obj.data = params(obj.data);
        if (obj.async === true) {
            xhr.onreadystatechange = function () {
                if (xhr.readyState == 4) {
                    callback();
                }
            };
        }
        if (obj.method === 'get'){
            obj.url += obj.url.indexOf('?') == -1 ? '?' + obj.data : '&' + obj.data;
        }
        xhr.open(obj.method, obj.url, obj.async);
        if (obj.method === 'post') {
            xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
            xhr.send(obj.data); 
        } else {
            xhr.send(null);
        }
        if (obj.async === false) {
            callback();
        }
        function callback() {
            if (xhr.status == 200) {
                obj.success(xhr.responseText);
            } else {
                console.log('error status:' + xhr.status + 'info:' + xhr.statusText);
            }   
        }
    }

使用：

    var oButton = document.getElementById('myButton');
    oButton.onclick = function() {
        ajax({
            method : 'post',
            url : 'xzavier',
            data : {
                'name' : 'xzavier',
                'sex' : 'man'
            },
            success : function (result) {
                console.log(result);
            },
            async : true
        });
    }

学习使用，要用于别处需要封装的还有很多。

### 后端实现

可以自学一点后端知识，便于学习。比如php，当然，现在node在前端这么火，怎么能不用呢。这里不多讲node安装、搭建项目等知识了，等之后自己再熟一点再写。

    var oButton = document.getElementById('myButton');
    var sName = document.getElementById('isName').value;
    oButton.onclick = function() {
        ajax({
            method : 'post',
            url : 'isuser',
            data : sName,
            success : function () {
                console.log('useable name');
            },
            async : false
        });
    }

node端：node学习（朴灵的书： 深入浅出nodeJs）

    //用户注册时判断用户名是否已存在
    app.post('/isuser', function(req, res) {
      var username = req.body.username;
      User.isUser(username, function(status){  //查询数据库，牵扯知识点多，不详述    
        if(status == 'OK'){
          res.send(200);          
        }else{
          res.send(404);
        }
      });
    });

### Ajax优缺点

Ajax带来的好处：

1、通过异步模式，实现动态不刷新，提升了用户体验   

2、优化了浏览器和服务器之间的传输，减少不必要的数据往返，减少了带宽占用  

3、Ajax在客户端运行，承担了一部分本来由服务器承担的工作，减少了大用户量下的服务器负载


Ajax的缺点：

1、Ajax不支持浏览器back按钮

2、安全问题,Ajax暴露了与服务器交互的细节  

3、对搜索引擎的支持比较弱

4、破坏了程序的异常机制

5、不容易调试

虽然优缺点，但是研发人员就是克服并完善技术缺点，发扬技术优点的存在O(∩_∩)O~




  [1]: https://segmentfault.com/a/1190000004356398#articleHeader18
