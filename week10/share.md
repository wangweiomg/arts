## ARTS - Share
### 关于reduce 函数

### 举例
我们知道reduce() 方法是对数组中的每个元素(从左到右)应用一个函数，将其减少为单个值。
例子：

```

	const array1 = [1, 2, 4, 6];
	const reducer = (acc, curr) =>  acc + curr;

	console.log(array1.reduce(reducer))
	
	// output
	13
	
	

```

### reduce 

####语法：
>arr.reduce(callback[, initialValue])

####参数：
callback: 执行数组中每个值的函数，包含四个参数

+ ```accumulator```
累加器累加回调的返回值; 它是上一次调用回调时返回的累积值，或initialValue（如下所示）。

+ ```currentValue```
数组中正在处理的元素。
+ ```currentIndex```可选
数组中正在处理的当前元素的索引。 如果提供了initialValue，则索引号为0，否则为索引为1。
+ ```array```可选
调用reduce()的数组


initialValue可选
用作第一个调用 callback的第一个参数的值。 如果没有提供初始值，则将使用数组中的第一个元素。 在没有初始值的空数组上调用 reduce 将报错。

####返回值
函数累计处理的结果


**注意：如果没有提供initialValue，reduce 会从索引1的地方开始执行 callback 方法，跳过第一个索引。如果提供initialValue，从索引0开始。**


### 对象的 reduce
假如我们有这样一个对象：

```
const data = [{x:22}, {x:33}]

```
要对它的属性求和，那么写法如下：

```

data.reduce((pre, cur) => pre.x + cur.x)
data.map(e => e.x).reduce((a,b)=>a+b)

// 结果是 55


```
map就是对每个元素调用一下函数。


---
[1]Reduce https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce


