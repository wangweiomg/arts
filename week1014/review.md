## ARTS - Review
## [你应该避免使用的3个JavaScript性能错误](https://hackernoon.com/3-javascript-performance-mistakes-you-should-stop-doing-ebf84b9de951)

作者：Yotam Kadishay 
全栈软件工程师 

来自：https://hackernoon.com/3-javascript-performance-mistakes-you-should-stop-doing-ebf84b9de951


如果我告诉你你知道的一切都是谎言，如果你了解到这些年发布的一些我们热爱的ECMAScript的关键特性确实存在危险的性能陷阱，糖衣包裹在华丽的一行回调代码之上，那么事情会变得怎么样？

这个故事开始于一些年前，回到ES5的天真年代...

我仍然生动的记得这一天，ES5发布了，伟大的新的数组方法被介绍给我们亲爱的JavaScript. 他们是forEach, reduce, map, filter -- 他们让我们觉得语言在成长，有更多功能，写代码变得更有趣平滑，更能方便的阅读和理解。

同时，一个新的环境生长了---Node.js， 它重新定义了全栈开发，给我们赋能从前端平滑过渡到后端。

如今，Node.js 在V8上使用最新的ECMAScript， 正试图被视作服务端开发语言大联盟的一部分，因此，它需要证明在性能上是有价值的。是的，有太多的参数需要考虑，是的，不存在能在所有方便都表现优异的银弹语言。但是，是否正在使用的开箱即用的功能编写JavaScript，如上面提到的数组函数对你的应用性能是有帮助的，还是有害的？

此外，客户端javascript声称去做一个不仅仅是视图的合理的解决方案，随着终端电脑用户增长的更强，网络更快，还要在我们的应用程序需要更快性能时候可以作为一个非常大的复合的一个依靠。

为了测试这个问题，我尝试比较一些脚本，深入了解我得到的结果。我在Chrome浏览器 v10.11.0 的Node.js执行以下测试,都是在macOS系统。

### 1. 数组循环
想到的第一个脚本是计算一个10K个项目的数组的和，这是一个有效的现实生活中的解决方案，我曾在尝试获取数据库中长表项目时候偶然发现，并使用总和来增强它，而不需要对数据库进行额外的查询。

我比较了对随机 10K个项目进行for , for-of, while, forEach, 和 reduce 求和. 运行测试 10000次，返回了以下结果：

```
For Loop, average loop time: ~10 microseconds
For-Of, average loop time: ~110 microseconds
ForEach, average loop time: ~77 microseconds
While, average loop time: ~11 microseconds
Reduce, average loop time: ~113 microseconds

```

当谷歌搜索怎么计算一个数组时候，reduce 是最先被提供的解决方案，但是却是最慢的。我的forEach没有特别好。甚至最新的for-of (ES6) 性能表现差劲。这说明，老的for 循环（和while） 提供了最好的性能 - 10倍不止。

为什么最新的和最推荐的解决方案让JavaScript 这么慢？造成这种痛苦来自于两个主要原因，reduce和forEach需要执行一个回调函数，这个函数被递归调用，使得堆栈膨胀，以及对执行代码进行额外的操作和验证。

### 2. 复制一个数组

虽然听起来像一个不太有趣的场景，这是不可变函数的支柱，当输出时候不会修改输入。

此处性能测试结果再次显示了同样有趣的倾向--当复制10K个10K数组的随机项目，用老的解决方案更快。同样最时髦的ES6 传播操作 `[...arr]` 和 Array from `Array.from(arr)` 加上 ES5 的map `arr.map(x=>x)` 都不如老将 slice `arr.slice()` 和 连接 `[].concat(arr)`

```
Duplicate using Slice, average: ~367 microseconds
Duplicate using Map, average: ~469 microseconds
Duplicate using Spread, average: ~512 microseconds
Duplicate using Conct, average: ~366 microseconds
Duplicate using Array From, average: ~1,436 microseconds
Duplicate manually, average: ~412 microseconds
```

### 3. 遍历对象

另一个频繁的场景是遍历对象，这是当我们尝试遍历JSON和对象时候的主要方式，而不是寻找特定的键值时候。同样老的解决方案像 for-in `for(let key in obj)`，或者比较新的 `Object.keys(obj)`(es6发布的) 还有`Object.entries(obj)`(来自ES8)都同样返回key和value.

对10K个项目的遍历性能分析，每个都包含1000个随机key和value, 用上面的方法表现如下：

```
Object iterate For-In, average: ~240 microseconds
Object iterate Keys For Each, average: ~294 microseconds
Object iterate Entries For-Of, average: ~535 microseconds

```
造成这样的原因是在后面的两个解决方案中是对数组值创建了枚举，而不是直接遍历对象不用key数组。但是最下面的结果仍然造成困扰。

### 结论

我的结论是清晰的--- 如果追求快速的性能表现是你的应用关键，或者如果你你的服务需要处理一些加载 --- 用最酷的，更可读的，清晰的选项将对你的应用性能产生重大影响--- 速度可以提高十倍！

下次，在闭眼采用华而不实的最新特性之前，确认他们也符合你的要求 --- 针对一个小的应用，写的更快更可读的代码是完美的--但是对与压力大的服务器和巨大的客户端应用，或许不是最好的实践。