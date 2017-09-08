## JavaScript-数组去重由慢到快由繁到简演化

### indexOf去重

    Array.prototype.unique1 = function() {
      	var arr = [];
      	for (var i = 0; i < this.length; i++) {
        	var item = this[i];
    	    if (arr.indexOf(item) === -1) {
    	      	arr.push(item);
    	    }
      	}
      	return arr;
    }
    [1,2,3,'4',3,4,3,1,'34',2].unique1(); //[1, 2, 3, "4", 4, "34"]
    
    //filter+indexOf写法，箭头函数为ES6新写法。

    Array.prototype.unique1 = function() {
        return this.filter((item, index, arr) => arr.indexOf(item) === index);
    }

indexOf的思想就是遍历一个数组的字符，判断这个字符在另一个数组存不存在，不存在就把这个字符也弄一个到结果数组里去。在 IE6-8 下，数组的 indexOf 方法还不存在（虽然这已经算有点古老的话题了O(∩_∩)O~），但是，程序员就要写一个indexOf方法：

    var indexOf = [].indexOf ? function(arr, item) {
      	return arr.indexOf(item);
    } :
    function indexOf(arr, item) {
      	for (var i = 0; i < arr.length; i++) {
        	if (arr[i] === item) {
          		return i;
        	}
      	}
      	return -1;
    }
     
    Array.prototype.unique2 = function() {
      	var arr = [];
      	for (var i = 0; i < this.length; i++) {
        	var item = this[i];
        	if (arr.indexOf(item) === -1) {
          		arr.push(item);
        	}
      	}
      	return arr;
    }
    [1,2,3,'4',3,4,3,1,'34',2].unique2(); //[1, 2, 3, "4", 4, "34"]

indexOf还可以以这样的去重思路：判断当前字符在数组中出现的位置是不是第一次出现的位置，如果是就把字符放到结果数组中。在去重过程中，原数组都是不变的。

    Array.prototype.unique3 = function(){
    	var arr = [this[0]]; 
    	for(var i = 1; i < this.length; i++){
    		if (this.indexOf(this[i]) == i){
    			arr.push(this[i]);
    		} 
    	}
    	return arr;
    }
    [1,2,3,'4',3,4,3,1,'34',2].unique3(); //[1, 2, 3, "4", 4, "34"]

### hash去重

以上indexOf正确性没问题，但性能上，两重循环会降低性能。那我们就用hash。

    Array.prototype.unique4 = function() {
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
    [1,2,3,'4',3,4,3,1,'34',2].unique4(); //[1, 2, 3, "4", 4, "34"]

hash去重的核心是构建了一个 hash 对象来替代 indexOf。以空间换时间。注意在 JavaScript 里，对象的键值只能是字符串，因此需要var key = typeof(item) + item 来区分数值 1 和字符串 '1' 等情况。（当然，ES6提供了Map数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。也就是说，Object结构提供了“字符串—值”的对应，Map结构提供了“值—值”的对应，是一种更完善的Hash结构现。）

那如果你想要'4' 和 4 被认为是相同的话（其他方法同理）

    Array.prototype.unique5 = function(){
        var arr=[];
        var hash={};
        for(var i=0,len=this.length;i<len;i++){
            if(!hash[this[i]]){ 
                arr.push(this[i]);
                hash[this[i]]=true;
            }
        }
        return arr;
    }
    [1,2,3,'4',3,4,3,1,'34',2].unique5(); //[1, 2, 3, "4", "34"]

### 排序后去重

    Array.prototype.unique6 = function(){
    	this.sort();
    	var arr = [this[0]];
    	for(var i = 1; i < this.length; i++){
    		if( this[i] !== arr[arr.length-1]){
    			arr.push(this[i]);
    		}
    	}
    	return arr;
    }
    [1,2,3,'4',3,4,3,1,'34',2].unique6(); //[1, 2, 3, "34", "4", 4]

先把数组排序，然后比较相邻的两个值，排序的时候用的JS原生的sort方法，所以非常快。而这个方法的缺陷只有一点，比较字符时按照字符编码的顺序进行排序。所以会看到10排在2前面这种情况。不过在去重中不影响。不过，解决sort的这个问题，是sort方法接受一个参数，这个参数是一个方法：

    function compare(value1,value2) {
        if (value1 < value2) {
            return -1;
        } else if (value1 > value2) {
            return 1;
        } else {
            return 0;
        }
    }
    [1,2,5,2,10,3,20].sort(compare);  //[1, 2, 2, 3, 5, 10, 20]

### Set去重

ES6提供了新的数据结构Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。现在浏览器正在全面支持，服务端的node也已经支持。
    Array.prototype.unique7 = function(){
        return [...new Set(this)];
    }
    [1,2,3,'4',3,4,3,1,'34',2].unique7(); //[1, 2, 3, "4", 4, "34"]
    
### 方法库

推荐一个方法库Underscore.js，在node或浏览器js中都很受欢迎。

    const _ = require('underscore');
    _.uniq([1, 2, 1, 3, 1, 4]);  //[1, 2, 3, 4]

### 测试时间

以上方法均可以用一个简单的方法去测试一下所耗费的时间,然后对各个方法做比较择优：

    console.time("test");
    [1,2,3,'4',3,4,3,1,'34',2].unique7();
    console.timeEnd("test");
    ==> VM314:3 test: 0.378ms
    
让数据变得大一点，就随机创建100万个数：

    var arr = [];
    var num = 0;
    for(var i = 0; i < 1000000; i++){
        num = Math.floor(Math.random()*100);
        arr.push(num);
    }
    console.time("test");
    arr.unique7();
    console.timeEnd("test");
    ==> VM325:3 test: 108025.815ms （比较数目越多，差距越大，更好选择）
    
我们平时使用数组去重的地方，视业务不同，需求量不一样。但使用的方法则可以视业务场景而选择一个正确的合适的方法来写代码。更重要的是我们的代码要写来让别人看得懂...写晦涩难懂的代码切不做注释只是装得一手好逼。。。
