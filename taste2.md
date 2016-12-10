##JavaScript-排序算法及简易优化
###快速排序
原理：在待排序序列中选一个分割元素，将待排序序列分隔成独立的子序列，子序列1里的元素比分割元素元素都小（大），子序列2反之，递归进行此操作，以达到子序列都有序。最后将子序列用concat方法连接起来即是排序好的序列。

    function quickSort(arr){
        if(arr.length <= 1){
            return arr;
        }    
        var num = Math.floor(arr.length/2);
        var numValue = arr.splice(num,1);
    
        var left = [];
        var right = [];
        for(var i = 0;i<arr.length;i++){
            if(arr[i] < numValue){
                left.push(arr[i]);
            }else{
                right.push(arr[i]);
            }
        }
        return quickSort(left).concat(numValue,quickSort(right));
    
    }
    console.log(quickSort([1,45,43,4,56,7,20,1]));
    //[1, 1, 4, 7, 20, 43, 45, 56]
优化：当待排序序列长度N < 10时，使用插入排序，可以加速排序。

    function quickSort(arr){
        if(arr.length <= 1){
            return arr;
        }
        if(arr.length < 10){
            insertSort(arr);
        }    
        var num = Math.floor(arr.length/2);
        var numValue = arr.splice(num,1);
    
        var left = [];
        var right = [];
        for(var i = 0;i<arr.length;i++){
            if(arr[i] < numValue){
                left.push(arr[i]);
            }else{
                right.push(arr[i]);
            }
        }
        return quickSort(left).concat(numValue,quickSort(right));
    
    }
    console.log(quickSort([1,3,4,645,43,4,56,333,44,564,7,20,1]));
    //[1, 1, 3, 4, 4, 7, 20, 43, 44, 56, 333, 564, 645]

###插入排序
原理：通过构建有序序列，对于未排序元素，在已排序序列中从后向前扫描，找到相应位置并插入。一般可以将第一个元素作为有序序列，用未排序的元素与之相比，插入，直到排序完毕。

    function insertSort(arr){
        var len = arr.length;
        if(len <= 1){
            return arr;
        }
        for(var i = 1;i<len;i++){
            var tmp = arr[i];
            var j = i;   
            while(arr[j-1] > tmp){
                arr[j] = arr[j-1];
                --j;
            }
            arr[j] = tmp;
        }
        return arr;
    }
    console.log(insertSort([1,45,43,4,56,7,20,1]));
    //[1, 1, 4, 7, 20, 43, 45, 56]
优化：二分插入排序（在有序序列中使用二分查找法查找一个插入位置，减少元素比较次数）和希尔排序（把序列分为很多小序列，对小序列直接插入排序，最后再整个插入排序）（下面演示二分插入排序）

    function binaryInsertSort(arr){
    	var len = arr.length;
        if(len <= 1){
            return arr;
        }
        for (var i = 1; i < len; i++) {
        	var tmp = arr[i], left = 0, right = i - 1;
        	while (left <= right) {
    	      	var index = parseInt((left + right) / 2);
    	      	if (tmp < arr[index]) {
    	      		right = index - 1;
    	      	} else {
    	        	left = index + 1;
    	      	}
        	}
    	    for (var j = i - 1; j >= left; j--) {
    	      	arr[j + 1] = arr[j];
    	    }
        	arr[left] = tmp;
      	}
    	return arr;
    }
    console.log(binaryInsertSort([1,45,43,4,56,7,20,1,2,3,4,56,3]));
    //[1, 1, 2, 3, 3, 4, 4, 7, 20, 43, 45, 56, 56]

###选择排序
原理：在待排序序列中找到最小（大）元素，放在序列的起始位置，然后，再从剩余元素中寻找最小（大）元素，然后放到已排序序列的末尾。重复，直到所有元素均排序完毕。

    Array.prototype.selectionSort = Array.prototype.selectionSort || function(){
        var len = this.length;
        if(len <= 1){
            return this;
        }    
        var min,tmp;
        for(var i = 0;i<len;i++){
            min = i;
            for(var j = i+1;j<len;j++){
                if(this[j] < this[min]){
                    min = j;
                }
            }
            if(i != min){
                tmp = this[i];
                this[i] = this[min];
                this[min] = tmp;
            }
        }
        return this;
    }
    console.log([1,45,43,4,56,7,20,1].selectionSort());
    //[1, 1, 4, 7, 20, 43, 45, 56]
优化：堆排序，在直接选择排序中，为了从序列中选出关键字最小（最大）的记录，必须进行n-1次比较，接着在剩下序列中选出最小（最大）的元素，又需要做n-2次比较。但是，在n-2次比较中，有的比较可能在前面的n-1次比较中已经做过，但由于前一趟排序时未保留这些比较结果，所以后一趟排序时又重复执行了这些比较操作。堆积是一个近似完全二叉树的结构，并同时满足堆积的性质：即子结点的键值或索引总是小于（或者大于）它的父节点。堆排序可通过树形结构保存部分比较结果，可减少比较次数。

    function heapSort(arr) { 
     	function swap(arr, i, j) {
    	  	var temp = arr[i];
    	  	arr[i] = arr[j];
    	  	arr[j] = temp;
    	}
     
     	function maxHeapify(arr, index, heapSize) {
      		var iMax,iLeft,iRight;
      		while (true) {
       			iMax = index;
       			iLeft = 2 * index + 1;
       			iRight = 2 * (index + 1);
     
       			if (iLeft < heapSize && arr[index] < arr[iLeft]) {
        			iMax = iLeft;
       			}
     
       			if (iRight < heapSize && arr[iMax] < arr[iRight]) {
        			iMax = iRight;
       			}
     
       			if (iMax != index) {	
        			swap(arr, iMax, index);
        			index = iMax;
       			} else {
       				 break;
       			}
      		}
     	}
     
     	function buildMaxHeap(arr) {
      		var i,iParent = Math.floor(arr.length / 2) - 1;
     
      		for (i = iParent; i >= 0; i--) {
       			maxHeapify(arr, i, arr.length);
      		}
     	}
     
     	function sort(arr) {
      		buildMaxHeap(arr);
     
      		for (var i = arr.length - 1; i > 0; i--) {
       			swap(arr, 0, i);
       			maxHeapify(arr, 0, i);
      		}
      		return arr;
     	}
     
     	return sort(arr);
    }
    console.log(heapSort([1,45,43,4,56,7,20,1,2,3,4,56,3]));
    //[1, 1, 2, 3, 3, 4, 4, 7, 20, 43, 45, 56, 56]
   
###冒泡排序
原理：从第一个元素开始，一次比较两个元素，如果arr[n]大于arr[n+1]，就交换。重复遍历直到没有再需要交换，排序完成。

    function bubbleSort(arr){
        var len = arr.length;
        if(len <= 1){
            return arr;
        }
        var tmp;
        for(var i = 0;i<len;i++){
            for(var j =0;j<len-1-i;j++){
                if(arr[j+1] < arr[j]){
                    tmp = arr[j];
                    arr[j] = arr[j+1];
                    arr[j+1] = tmp;
                }
            }
        }
        return arr;
    }
    console.log(bubbleSort([1,45,43,4,56,7,20,1]));
    //[1, 1, 4, 7, 20, 43, 45, 56]

优化：上面代码中，里面一层循环在某次扫描中没有发生交换，说明此时数组已经全部排好序，后面的步骤就无需再执行了。因此，增加一个标记，每次发生交换，就标记，如果某次循环完没有标记，则说明已经完成排序。

    function bubbleSort(arr)  {  
        var len = arr.length;
        if(len <= 1){
            return arr;
        }
        var bSwaped = true;  
        for (var i = 0; i < len -1; i++)  {  
            // 每次先重置为false  
            bSwaped = false;  
            for (var j = len - 1; j > i ; j--)  {  
                if (arr[j-1] > arr[j])  {  
                    var temp = arr[j-1];  
                    arr[j-1] = arr[j];  
                    arr[j] = temp;  
      
                    bSwaped = true;  
                }  
            }  
            // 如果上一次扫描没有发生交换，则说明数组已经全部有序，退出循环  
            if (!bSwaped){
                break;
            }              
        }  
        return arr;
    }  
    console.log(bubbleSort([1,45,43,4,56,7,20,1]));
    //[1, 1, 4, 7, 20, 43, 45, 56]

在上一步优化的基础上进一步思考：如果R[0..i]已是有序区间，上次的扫描区间是R[i..n]，记上次扫描时最后一次发生交换的位置是lastSwapPos，那么lastSwapPos会在在i与n之间，所以R[i..lastSwapPos]区间是有序的，否则这个区间也会发生交换；所以下次扫描区间就可以由R[i..n]缩减到[lastSwapPos..n] :

    function bubbleSort(arr){ 
    	var len = arr.length;
        if(len <= 1){
            return arr;
        } 
        var lastSwapPos = 0, lastSwapPosTemp = 0;  
        for (var i = 0; i < len - 1; i++){  
            lastSwapPos = lastSwapPosTemp;  
            for (var j = len - 1; j > lastSwapPos; j--){  
                if (arr[j - 1] > arr[j]){  
                    var temp = arr[j - 1];  
                    arr[j - 1] = arr[j];  
                    arr[j] = temp;  
      
                    lastSwapPosTemp = j;  
                }  
            }  
            if (lastSwapPos == lastSwapPosTemp){
            	break;
            }                         
        }  
        return arr;
    }
    console.log(bubbleSort([1,45,43,4,56,7,20,1]));
    //[1, 1, 4, 7, 20, 43, 45, 56]

这一些列优化都需要测速才知道有没有优化成功，只是简单的测试一两个数组是不容易看出来的。我们可以造一些很大的数据去测试，再用一个比较简单的测试时间的方法，随机创建10万个数：

    var arr = [];
    var num = 0;
    for(var i = 0; i < 100000; i++){
        num = Math.floor(Math.random()*100000);
        arr.push(num);
    }
    console.time("testTime");
    bubbleSort(arr);
    console.timeEnd("testTime");
    ==> testTime: 21900.684ms （比较数目越多，差距越大，更好对比）
