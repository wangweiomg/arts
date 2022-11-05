## ARTS - Review
## [为什么Java小伙对Node.js和JavaScript 如此兴奋？](https://blog.sourcerer.io/why-is-a-java-guy-so-excited-about-node-js-and-javascript-7cfc423efb44)

-- David Herron , 软件工程师和作家，热衷于Node.js 以及清洁能源技术。Node.js WEB 开发网站的作者。https://sourcerer.io/robogeek


在Sun Microsystems 公司JavaSE 组工作超十年的人不应该牺牲Java字节码，并将抽象接口实例化进行到底吗？对这个前JavaSE组成员，在2011年学习Node.js平台就像呼吸了新空气。在被Sun公司在2009年一月份被解雇(仅仅被甲骨文收购前)后，我学习并迷上了Node.js

有多着迷？从2010年，我写了大量的Node.js编程的文章。即，Node.js Web开发四个版本，再加上Node.js编程的其他书籍和众多博客教程帖子。用大量的时间阐述Node.js和JavaScript语言的先进性。

在Sun microsystems 工作时，我信任Java里的一切。我在JavaONE 大会提出了会话，共同开发了 java.awt.Robot 类，运行了Mustang Regressions 竞赛（Java 1.6 发行版的Bug追踪竞赛），帮助启动了“Java发行许可证”，这是linux发行版 预先的Jdk的方案，之后在发行正式openjdk扮演了个小角色。一路上我在 java.net(现在已废弃) 博客上，一周写两篇文章，坚持了6年讨论Java生态。
一个很大的主题就是捍卫Java语言和预言Java要死的言论斗争。

和Java字节码共同生活会是怎样？我在这里的解释就是，怎么作为Java倡导者直到死去，就怎么作为Node.js/JavaScript 倡导者直到死去。

这并不是说我完全脱离Java。过去3年我写了一堆重要的 Java/Spring/Hibernate代码。当我全情投入工作-- 我在 Solar Industry 做精神满足的事情像写kiloWatt-hours的数据库查询 -- Java编码失去了热情。

两年的 Spring 编码教会了一个清晰的道理，复杂的书写不会产生简单，只会产生复杂。

+ Java 充满公式化的代码，掩盖了程序员的意图。
+ Spring 和 SpringBoot 教会了， 复杂产生更多的复杂
+ JavaEE 是一个 设计委员会设计的工程，覆盖了企业应用开发的一切，因此很复杂
+ Spring的编程体验是很好的，直到有一天一个难以理解的没听过的异常在子系统里爆出来，定位问题就需要3天以上
+ 框架允许编码器编写0代码的开销是多少？
+ 当IDE 像Eclipse越来越强大，就是Java复杂的一个征兆。
+ Node.js 就是一个轻量级事件驱动框架的结晶。
+ JavaScript社区就像乐于移除模板，让程序员可以表达自己的意图。
+ 回调地狱的解决方案，就是异步/等待 函数，就是移除样板的例子，让程序更能展示意图。
+ 用Node.js 编码就是乐趣。
+ JavaScript 缺乏Java的严格类型检查。这是双刃剑，代码更易于编写，但需要更多测试确保正确。
+ npm /yarn 包管理系统 用着很有趣， 对应Java是Maven
+ Java和Node.js 都提供了卓越性能。与JavaScript慢所以Node.js就慢的观念背道而驰。
+ Node.js的性能是搭载着 用来提高Chrome浏览器性能的谷歌开发的V8引擎上。
+ 浏览器之间的竞争使得JavaScript越来强大，利于Node.js

### Java成为了负担，Node.js编码充满乐趣
一些工具或对象是设计师多年设计的精华。他们尝试不同想法，删除非必要的属性，最后只剩下必须的。常常这些对象很强大且简单令人激动，但Java不是这个路子。

Spring 是Java web开发的流行框架。Spring，尤其Spring Boot 的核心期望是更易于使用预先配置的JavaEE栈。Spring 程序员不必要处理所有的 Servlets ，数据持久化，应用服务器，谁知道还有其他啥东西，就可以得到一个完整的系统。Spring替你关心这些所有的细节，你只需要关注编码。例如， JPA Repository 类就能通过类似方法“findUserByFirstName” 来合成数据库查询。 ---你不需要写任何代码，简简单单的用Repository 格式的方法命名，然后Spring处理剩下的一切。

这是一个伟大的故事，优美的体验，直到破灭。

当你发现一个Hibernate 持久对象异常“detached entity passed to persist” 时候，代表什么意思呢？ 花费几天来排错 -- 过度简化的风险 -- 它意味着 REST端点的JSON ID属性包含值。Hibernate，过度简化，想要控制ID 的值，抛出这个令人困惑的异常。有上千个类似的令人困惑不清的异常信息。当子系统用Spring 栈之后，就像一个复仇者等着你犯错，然后用应用程序突然崩溃的意外来袭击你。

然后大量追踪堆栈。然后好几屏的全是抽象方法。Spring显然解决了实现代码所需的配置。这种级别的抽象明显很多逻辑去寻找一切执行请求。一个长的堆栈追踪并不一定是坏的。反而指出一个内存/性能 开销的大小。

当程序员写0代码时候 findUserByFirstName 会如何执行呢？ 框架不得不解析方法名， 猜测程序员的意图，构造像语法树一样的东西，生成SQL等。这一切的开销是多少?这样编码人就不用编码了吗？

经过几十次这样的学习后，花上几星期时间成本学习你不本不该学的奥秘，你可能会得出和我一样的结论：把复杂写出来并不会产生简单，只会产生更多复杂。

转向Node.js 的点。

### 另一方面说Node.js
Spring 和JavaEE 及其复杂的地方， Node.js 就像呼吸新空气。首先是Ryan Dahl 在开发Node.js核心平台式使用了设计美学。Dahl过去经验是用多个线程制作了一个重量级的复杂系统。他认为有些不一样的地方，花费两年在Node.js 添加很多精品。结果就是一个轻量级系统，一个单个执行线程，一个精巧的JavaScript 匿名异步回调函数， 一个巧妙的实现异步的运行库。最初的定位是用是用事件传递给回调函数实现高吞吐量事件处理。

然后就是JavaScript 语言本身。JavaScript程序员似乎有取出样公式化的美感，所以程序员的意图可以清晰显示出来。

对比Java和JavaScript 实现监听者方法的实现例子是， 在Java Listeners 需要创建一个抽象接口类的实体。这需要大量的语法掩盖正在发生的事。程序员如何在看到模板的面纱后面？

在JavaScript 用简单的匿名函数 - 闭包。 不需要寻找正确的抽象接口。相反，你只需要编写所需代码，没有多余冗长。

另外的：很多编程语言模糊了程序猿的意图，让代码难懂。

这一点在Node.js 上，我们必须指出：回调地狱。


### 解决方案有时会出现自己的问题

JavaScript 异步编程我们长期有两个困扰问题。一个在JavaScript是什么时候回变成 回调地狱。 很容易就陷入回调函数深层嵌套， 每个级别的复杂嵌套代码，都可能产生错误影响全局。一个相关的问题是JavaSCript语言不能帮助程序员正确的表达异步执行。

几个库涌现出来，希望简化异步执行。另一个关于复杂性的例子，创造了更多复杂性。

给个例子：

```

const async = require(‘async’);
const fs = require(‘fs’);
const cat = function(filez, fini) {
  async.eachSeries(filez, function(filenm, next) {
    fs.readFile(filenm, ‘utf8’, function(err, data) {
      if (err) return next(err);
      process.stdout.write(data, ‘utf8’, function(err) {
        if (err) next(err);
        else next();
      });
    });
  },
  function(err) {
    if (err) fini(err);
    else fini();
  });
};
cat(process.argv.slice(2), function(err) {
  if (err) console.error(err.stack);
});
```

这个简单的应用就是对UNIX 程序Cat 命令的简单模仿。
async 库天才的简化异步执行序列。但是它使用需要一堆样板代码掩盖程序员的意图。





