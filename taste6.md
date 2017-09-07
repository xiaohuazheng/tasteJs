## JavaScript-DOM

### DOM简介

DOM（Document Object Model）即文档对象模型，针对HTML 和XML 文档的API（应用程序接口）。DOM 描绘了一个层次化的节点树，运行开发人员添加、移除和修改页面的某一部分。通过 DOM，可以访问所有的 HTML 元素，连同它们所包含的文本和属性。可以对其中的内容进行修改和删除，同时也可以创建新的元素。document对象是文档的根节点，window.document属性就指向这个对象。

### DOM节点

分为元素节点、属性节点和文本节点。

而这些节点又有三个非常有用的属性，分别为：nodeName、nodeType 和nodeValue

    节点类型       nodeName     nodeType     nodeValue
      元素          元素名称       1           null
      属性          属性名称       2          属性值
      文本           #text        3     文本内容(不包含html)

F12打开console，即可操作当前网页节点：

    document.getElementById('mainLike').nodeName  //"BUTTON"
    document.getElementById('mainLike').nodeType  //1
    document.getElementById('mainLike').nodeValue //null
    document.getElementById('mainLike').getAttributeNode("class").nodeName //"class"
    document.getElementById('mainLike').getAttributeNode("class").nodeType //2
    document.getElementById('mainLike').getAttributeNode("class").nodeValue //"btn btn-success btn-lg mr10"
    document.getElementById('mainLike').firstChild.nodeName  //"#text" 对于所有文本节点nodeName都是"#text"
    document.getElementById('mainLike').firstChild.nodeType  //3
    document.getElementById('mainLike').firstChild.nodeValue  //"0 推荐"

### DOM元素选择

          方法                      说明                  备注
    getElementById()          获取特定ID元素的节点       获取单个节点对象
    getElementsByClassName()  获取指定class类的节点列表  返回值为节点类数组
    getElementsByTagName()    获取相同元素的节点列表     返回值为节点类数组
    getElementsByName()       获取相同名称的节点列表     返回值为节点类数组
    querySelector             获取class第一个或id的节点  返回值为一个节点对象
    querySelectorAll          获取class或id的节点列表    返回值为节点类数组

jQuery在选择器上做的非常好，使用的选择器引擎Sizzle占了jQuery很大一部分。延伸：[jQuery选择器浅析][1]

querySelectorAll 和getElementsBy获取节点数组系列方法有一个很重要的区别：

`querySelectorAll` 返回的是一个 `Static Node List`

`getElementsBy`系列的返回的是一个 `Live Node List`

具体可参见[知乎提问，答案很详细][2]
    

### 获取HTML元素属性

    属性         说明
    id        元素节点的id 名称
    title     元素节点的title 属性值
    style     CSS 内联样式属性值
    className CSS 元素的类
    
    document.getElementById('xzavier').id; //获取id
    document.getElementById('xzavier').id = 'man'; //设置id
    document.getElementById('xzavier').title; //获取title
    document.getElementById('xzavier').title = '标题' //设置title
    document.getElementById('xzavier').style; //获取CSSStyleDeclaration对象
    document.getElementById('xzavier').style.color; //获取style对象中color的值
    document.getElementById('xzavier').style.color = 'red'; //设置style对象中color的值
    document.getElementById('xzavier').className; //获取class
    document.getElementById('xzavier').className = 'xzavier'; //设置class

### 属性方法
    getAttribute()            获取特定元素节点属性的值
    setAttribute()            设置特定元素节点属性的值
    removeAttribute()         移除特定元素节点属性

#### getAttribute()方法

getAttribute()方法将获取元素中某个属性的值。它和直接使用.属性获取属性值的方法有一定区别。

    document.getElementById('xzavier').getAttribute('id');//获取元素的id 值
    document.getElementById('xzavier').id; //获取元素的id 值
    document.getElementById('xzavier').getAttribute('mydiv');//获取元素的自定义属性值
    document.getElementById('xzavier').mydiv //获取元素的自定义属性值，非IE 不支持
    document.getElementById('xzavier').getAttribute('class');//获取元素的class 值，IE 不支持
    document.getElementById('xzavier').getAttribute('className');//非IE 不支持

这里说一下attribute和property的区别，基本可以总结为attribute节点都是在HTML代码中可见的，而property只是一个普通的名值对属性。

Property：属性，所有的HTML元素都由HTMLElement类型表示，HTMLElement类型直接继承自Element并添加了一些属性，添加的这些属性分别对应于每个HTML元素都有下面的这5个标准特性: id,title,lang,dir,className。DOM节点是一个对象，因此，他可以和其他JavaScript对象一样添加自定义的属性以及方法。property的值可以是任何的数据类型，对大小写敏感，自定义的property不会出现在html代码中，只存在js中。

Attribute：特性，区别于property，attribute只能是字符串，大小写不敏感，出现在innerHTML中，通过类数组attributes可以罗列所有的attribute。

相同之处

标准的 DOM properties 与 attributes 是同步的。公认的（非自定义的）特性会被以属性的形式添加到DOM对象中。如，id，align，style等，这时候操作property或者使用操作特性的DOM方法如getAttribute()都可以操作属性。不过传递给getAttribute()的特性名与实际的特性名相同。因此对于class的特性值获取的时候要传入“class”。

不同之处

1).对于有些标准的特性的操作，getAttribute与点号(.)获取的值存在差异性。如href，src，value，style，onclick等事件处理程序。

2).href：getAttribute获取的是href的实际值，而点号获取的是完整的url，存在浏览器差异。

#### setAttribute()方法

setAttribute()方法将设置元素中某个属性和值。它需要接受两个参数：属性名和值。如果属性本身已存在，那么就会被覆盖。

    document.getElementById('xzavier').setAttribute('align','center');//设置属性和值
    document.getElementById('xzavier').setAttribute('xzavier','123456');//设置自定义的属性和值

#### removeAttribute()方法

removeAttribute()可以移除HTML 属性。

    document.getElementById('xzavier').removeAttribute('style');//移除属性

PS：IE6 及更低版本不支持removeAttribute()方法。

### 层次节点属性

节点的层次结构可以划分为：父节点与子节点、兄弟节点这两种。当我们获取其中一个元素节点的时候，就可以使用层次节点属性来获取它相关层次的节点。

       属性                说明
    childNodes      获取当前元素节点的所有子节点
    firstChild      获取当前元素节点的第一个子节点
    lastChild       获取当前元素节点的最后一个子节点
    ownerDocument   获取该节点的文档根节点，相当与document
    parentNode      获取当前节点的父节点
    previousSibling 获取当前节点的前一个同级节点
    nextSibling     获取当前节点的后一个同级节点
    attributes      获取当前元素节点的所有属性节点集合

### 节点操作

          方法                说明
    write()           把任意字符串插入到文档中
    createElement()   创建一个元素节点
    appendChild()     将新节点追加到子节点列表的末尾
    createTextNode()  创建一个文件节点
    insertBefore()    将新节点插入在前面
    repalceChild()    将新节点替换旧节点
    cloneNode()       复制节点
    removeChild()     移除节点

    document.write('<p>这是一个段落！</p>')';  //把任意字符串插入到文档中去
    var xzavier = document.getElementById('xzavier'); //获取某一个元素节点
    var p = document.createElement('p'); //创建一个新元素节点<p>
    xzavier.appendChild(p); //把新元素节点<p>添加子节点末尾
    var text = document.createTextNode('段落'); //创建一个文本节点
    p.appendChild(text); //将文本节点添加到子节点末尾
    xzavier.parentNode.insertBefore(p, xzavier); //把<div>之前创建一个节点
    xzavier.parentNode.replaceChild(p,xzavier); //把<div>换成了<p>
    var clone = xzavier.firstChild.cloneNode(true); //获取第一个子节点，true 表示复制内容
    xzavier.appendChild(clone); //添加到子节点列表末尾
    xzavier.parentNode.removeChild(xzavier); //删除指定节点
    
### DOM操作内容

#### innerText

    document.getElementById('xzavier').innerText; //获取文本内容(如有html 直接过滤掉)
    document.getElementById('xzavier').innerText = 'xzavier'; //设置文本(如有html 转义)

除了Firefox 之外，其他浏览器均支持这个方法。但Firefox 的DOM3级提供了另外一个类似的属性：textContent，做上兼容即可通用。

    document.getElementById('xzavier').textContent; //Firefox 支持

兼容一下：
    function getInnerText(element) {
        return (typeof element.textContent == 'string') ? element.textContent : element.innerText;
    }
    function setInnerText(element, text) {
        if (typeof element.textContent == 'string') {
            element.textContent = text;
        } else {
            element.innerText = text;
        }
    }

#### innerHTML

    document.getElementById('xzavier').innerHTML; //获取文本(不过滤HTML)
    document.getElementById('xzavier').innerHTML = '<b>xzavier</b>'; //可解析HTML

虽然innerHTML 可以插入HTML，但本身还是有一定的限制，也就是所谓的作用域元素，离开这个作用域就无效了。

    xzavier.innerHTML = "<script>alert('xzavier);</script>"; //<script>元素不能被执行
    xzavier.innerHTML = "<style>background:red;</style>"; //<style>元素不能被执行

还有两个方法outerText，outerHTML基本不怎么用。最常用的innerHTML 属性和节点操作方法的比较，在插入大量HTML 标记时使用innerHTML 的效率明显要高很多。因为在设置innerHTML 时，会创建一个HTML 解析器。这个解析器是浏览器级别的(C++编写)，因此执行JavaScript 会快的多。但，创建和销毁HTML 解析器也会带来性能损失。最好控制在最合理的范围内。

### 获取元素大小

#### clientWidth 和clientHeight

这组属性可以获取元素可视区的大小，可以得到元素内容及内边距所占据的空间大小。

    xzavier.clientWidth; //400
    xzavier.clientHeight //400

返回了元素大小，但没有单位，默认单位是px

1.增加边框，无变化

2.增加外边距，无变化

3.增加滚动条，最终值等于原本大小减去滚动条的大小

4.增加内边距，最终值等于原本大小加上内边距的大小

#### scrollWidth 和scrollHeight

这组属性可以获取滚动内容的元素大小。

    xzavier.scrollWidth; //400
    xzavier.scrollWidth; //400

1.增加内边距，最终值会等于原本大小加上内边距大小

2.增加滚动条，最终值会等于原本大小减去滚动条大小

#### offsetWidth 和offsetHeight

这组属性可以返回元素实际大小，包含边框、内边距和滚动条。

    xzavier.offsetWidth;  //400
    xzavier.offsetHeight; //400

返回了元素大小，默认单位是px。如果没有设置任何CSS 的宽和高度，他会得到计算后的宽度和高度。

1.增加边框，最终值会等于原本大小加上边框大小

2.增加内边距，最终值会等于原本大小加上内边距大小

3.增加外边据，无变化

4.增加滚动条，无变化，不会减小

#### clientLeft 和clientTop

这组属性可以获取元素设置了左边框和上边框的大小。

    xzavier.clientLeft; //获取左边框的长度
    xzavier.clientTop; //获取上边框的长度
    
#### offsetLeft 和offsetTop

这组属性可以获取当前元素相对于父元素的位置。

    xzavier.offsetLeft; //20
    xzavier.offsetTop;  //20

获取元素当前相对于父元素的位置，最好将它设置为定位position:absolute；否则不同的浏览器会有不同的解释。加上边框和内边距不会影响它的位置，但加上外边据会累加。

#### scrollTop 和scrollLeft

这组属性可以获取滚动条被隐藏的区域大小，也可设置定位到该区域。

    xzavier.scrollTop;  //获取滚动内容上方的位置
    xzavier.scrollLeft; //获取滚动内容左方的位置

滚动条回顶部

    function scrollStart(element) {
        if (element.scrollTop != 0) element.scrollTop = 0;
    }
    
### document对象属性

         属性	                  说明
    document.title	         设置文档标题
    document.linkColor	     未点击过的链接颜色
    document.alinkColor	    激活链接的颜色
    document.vlinkColor	    已点击过的链接颜色
    document.URL	           设置URL属性
    document.fileSize	      文件大小，只读属性
    document.cookie	        设置和读取cookie
    document.charset	       设置字符集 

一般来说用的多的也就title，URL，cookie，charset等，其他的就不列了。


先写这么些，打篮球去了。代码，篮球，生活...


  [1]: https://segmentfault.com/a/1190000006667079
  [2]: https://www.zhihu.com/question/24702250
