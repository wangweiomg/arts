## ARTS - Tip

### 1. ES6 中 let 和 var

最近写js, IDE 提示推荐使用 let 或const 代替 var 声明变量，于是就了解了下ES6的 let和 var的区别。

> 关键词：

> + let : 变量只能声明一次
> + var : 变量可以多次声明
> 

```
var a = 5;
  var a = 3;
  let b = 2;
  let b = 4;  
  console.console.log(a);
  console.console.log(b); 
```
然后就报错了 Uncaught SyntaxError: Identifier 'b' has already been declared
    at <anonymous>:1:1
    
    
于是，在方法里调用方法，var也会有不一样的：

```

// var
    for(var i=0;i<5;i++){
           setTimeout(function(){
               console.log("var：" + i);
          })
    }
       // let
    for(let i=0;i<5;i++){
           setTimeout(function(){
               console.log("let" + i);
          })
    }



```

var 输出的结果是 5, let输出是 0,1,2,3,4。
如果用var实现let这个功能就需要用闭包了

```
for(var i = 0; i < 5; i++) {
	(function(i){
		setTimeout(function(){
			console.log("var: " + i);
		});
	})(i);

}


```


### 2. Safari 和 Chrome Date.parse 
有一个需求是页面显示 yyyy-MM-dd HH:mm:ss 格式的日期，于是，同时在js里 这个日期还要参与计算， 在转化为 Date对象时候，使用了  Date d = new Date('2018-08-06 12：20：13'); 
这个在Chrome可以转化正常，但是在Safari 里就报错了 Invalid Date 。 


解决办法： let d = new Date(Date.parse(dateString));



---

[1] var和let区别 https://www.jianshu.com/p/bf548e0ee60b
[2]