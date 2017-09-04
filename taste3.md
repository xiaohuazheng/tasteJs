## JavaScript--cookie

cookie可以像身份证一样在客户端请求服务器的时候确定信息。也可以在客户端分担服务端的压力，做很多判断和存储信息。

### cookie 优缺点

优点：

1.只在cookie中存放不敏感数据，即使被盗也不会有重大损失。

2.控制cookie的生命期，使之不会永远有效。就算被盗了偷盗者很可能拿到的是一个过期的cookie。

3.cookie帮助服务端承担了很大的压力，可以利用cookie在和客户端做很多判断而不应经过服务端。

4.极高的扩展性和可用性，使用简单，操作方法方便

缺点：

1.cookie数量和长度的限制。每个cookie长度不能超过4KB，否则会被截掉。IE下每个domain最多只能有50条cookie（IE6是20条），Firefox最多50个cookie，chrome和Safari没有做硬性限制，IE和Opera 会清理近期最少使用的cookie，Firefox会随机清理cookie。

2.安全性问题。这是cookie一个隐患，如果cookie被人拦截了，那人就可以取得所有的session信息。即使加密也与事无补，因为拦截者并不需要知道cookie的意义，他只要原样转发cookie就可以达到目的了。

3.有些状态不可能保存在客户端。例如，为了防止重复提交表单，我们需要在服务器端保存一个计数器。如果我们把这个计数器保存在客户端，那么它起不到任何作用。所以还是有一定的局限性。

### 设置cookie

一般主要设置cookie名字和值、cookie有效期、路径、域名、是否安全传输。

原生方法：

    document.cookie="key="+value;

封装方法：

    function setCookie(key, value, expires, path, domain, secure) {     
    	var cookieText = encodeURIComponent(key) + '=' + encodeURIComponent(value);     
    	if (expires instanceof Date) {         
    		cookieText += '; expires=' + expires;     
    	}     
    	if (path) {         
    		cookieText += '; expires=' + expires;     
    	}     
    	if (domain) {         
    		cookieText += '; domain=' + domain;     
    	}     
    	if (secure) {         
    		cookieText += '; secure';     
    	}     
    	document.cookie = cookieText; 
    } 
    

JQuery方法（JQuery没有封装cookie方法，需要下载基于JQuery的插件jquery.cookie.js）：

    $.cookie('key','value',{
        expires：7,
        path：'/',
        domain: 'xxx.com',
        secure: false
    });
   
### 获取cookie 

原生方法：

    var cookieStr = document.cookie;  //cookieStr=='username=Xzavier;password=123456;sex=man'

这样获得了所有的cookie，是一个字符串。根据需要选取，比如：

    var username=document.cookie.split(";")[0].split("=")[1];
    var password=document.cookie.split(";")[1].split("=")[1];
    

封装方法：

    function getCookie(key) {     
    	var cookieName = encodeURIComponent(key) + '=';     
    	var cookieStart = document.cookie.indexOf(cookieName);     
    	var cookieValue = null;     
    	if (cookieStart > -1) {         
    		var cookieEnd = document.cookie.indexOf(';', cookieStart);         
    		if (cookieEnd == -1) {             
    			cookieEnd = document.cookie.length;         
    		}         
    		cookieValue = decodeURIComponent(document.cookie.substring(cookieStart + cookieName.length, cookieEnd));     
    	}     
    	return cookieValue; 
    } 
    

JQuery方法：

    $.cookie(‘key’); //value?value:null
    

### 删除cookie

原生方法：

    document.cookie = "key=value;expires=" + new Date(0); //时间可以是现在以及现在之前

封装方法：

    function unsetCookie(key) {     
    	document.cookie = key + "= ; expires=" + new Date(0); 
    } 

JQuery方法：

    $.cookie(‘key’,null);

其他参数设置：
    $.cookie("key", value, {
        expires: new Date(0),
        path: '/',
        domain: 'xxx.com'
    });
    
cookie在持久保存客户端数据提供了方便，分担了服务器存储的负担，虽然有局限性，但是不可替代的。使用的方法也非常简单，但平时使用cookie的时候也需要多多注意安全性。

HTML5新的API包括localStorage和sessionStorage见本系列后面的文章。