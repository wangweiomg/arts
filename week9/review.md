## ARTS - Review

[为什么Java小伙对Node.js和JavaScript如此兴奋？（下）](https://blog.sourcerer.io/why-is-a-java-guy-so-excited-about-node-js-and-javascript-7cfc423efb44)

这是个循环。但不是用循环的写法，它不使用普通的循环结构。此外，错误和结果不会落入普通写法的循环里，但是会落入这种回调函数中。在ES2015/2016 特性加入Node.js之前，这是我们能做的极致了。

在Node.js 10.x 中等价于：

```

const fs = require('fs').promises;

async function cat(filenmz) {
	for (var filenm of filenmz) {
		let data = await fs.readFile(filenm, 'utf8')l
		await new Promise((resolve, reject) => {
			process.stdout.write(data, 'utf8', (err) => {
				if (err) reject(err);
				else resolve();
			});
		});
	}
}


cat(process.argv.slice(2)).catch(err => {
	console.error(err.stack);
});



```

这个重写之前的例子，使用了async/await 方法。和异步结构相同，但是是用普通循环结构写的。错误和结果用自然方式报告。很容易读，容易编写，容易了解程序员的意图。

唯一的缺点是 ``` process.stdout.write ``` 不提供 承诺接口 所以不能在没有包装承诺时候不能清晰的使用异步方法。


回调大坑问题并没有通过复杂书写解决。相反，语言范式的改变既解决了问题，也解决了临时方案造成的过度冗长。用 async 方法我们代码变得更优美。

虽然开始是针对Node.js 的一个点，但是优秀的解决方案将他转回为Node.js和JavaScript的一个点。


### 通过明确定义的类型和接口来确定清晰性

我在作为Java布道者时候的一个观点是严格类型检查能书写大的应用程序。那时的标准是开发整体系统（没有微服务，没有Docker等）。因为Java 有严格类型检查，Java编译器通过编译过程帮你避免很多bug。相比下，JavaScript 松散类型。结论很明显，程序员不知道自己得到什么样的对象，所以怎么知道该怎么做。

Java 的严格类型另一面更模板化。程序员不是在不断的类型转化就是在努力确定每步骤正确。编码者花费时间编码，及其精确，使用模板，希望早发现捕捉修复错误来节省时间。

问题如此庞大必须使用一个大型的复杂IDE。一个简单的程序员编辑器是不够的。唯一的方式让Java程序员保持理智的是下拉显示一个对象的可用属性，描述出方法的参数，帮助构造类，帮助重构，所有这些在开发工具Eclipse， NetBean， Intellij 都提供。

别人我从maven开始，多么可怕的工具。


JavaScript, 变量类型没有声明，类型转换通常不用。因此代码干净阅读，但是有未捕获的错误风险。

无论这点同意Java或者反对他， 取决于你看待的角度。我的观点十年前是这些限制对获得更多精确性是有价值的。我今天的观点是这让产生大量工作，用JavaScript方法来处理会很简单。

### 通过小的简单的测试模块来排除bug
Node.js 鼓励程序员把程序分成小单元模块。看起来是个小事，但是一定程度上让问题定位方便。

一个模块是：

+ 自包含 -- 意味着，它的包让代码在一个单元里
+ 强范围 -- 模块中代码很安全不会被其他地方的代码侵入
+ 明确的导出 -- 默认情况模块中的代码和数据不是被导出的，需要选择需要导出方法和数据给别的使用
+ 明确的导入 -- 模块声明他们依赖于哪些模块
+ 潜在的独立 -- 很容易就公开发布到npm仓库，或者私有发布模块在其他地方，让应用分享很简单
+ 容易阅读 -- 读少量的代码就能容易理解意图。
+ 容易测试 -- 如果实现正确，小模块很容易被单元测试

所有这些特点共同让Node.js模块容易测试有一个好的定义范围。

忧虑的是JavaScript缺乏严格类型检查代码可能容易导致错误。在一个小的有清晰边界模块中，受影响的代码范围很可能限制在那个模块中。这模块的边界保证了大部分的关注点很小，安全隐蔽。

另一个弱类型解决方案是增加测试。

你必须花费一些生产力去增加测试。你的测试方案必须覆盖可能编译出错的地方。你肯定要测试你的代码不是吗？

对于那些想要JavaScript静态类型检查的人，可以看TypeScript。 我们用过这门语言，但是听说过它的大名。它和JavaScript兼容，增加了有用的类型检查和其他特性。

以下是转向Node.js和JavaScript的有点：


### 包管理

我愤怒与Maven的包管理，几乎不能直接写出你要的。可能别人也不喜欢Maven， 或者轻视它，没有中间状态。

Java生态系统一个问题是没有一个有凝聚力的包管理系统。Maven包管理系统存在工作的很好，可能用Gradle也不错。但是并没有像Node.js一样的有用、好用、强大的包管理系统。

在Node.js世界有两个天才的包管理系统一起工作。第一个npm,和npm仓库，这类工具的独家。

用npm我们用很好的概要来表述包依赖。一个依赖可以被详细描述通过几个松散的分级方式，如"*"代表最近的版本。Node.js社区发布成千上万个包到npm仓库。使用外部包和npm仓库包是一样简单的。

npm包仓库如此好用不仅服务Node.js，也服务前端工程师。之前的工具像Bower包管理工具过时了。Bower已经淘汰了，现在很多人通过npm 仓库来找到他们需要的所有JavaScrpt库。很多前端工程师工具链，像Vue.js CLI 和 Webpack, 都是用Node.js写的。

另一个Node.js包管理系统是 yarn, 和npm仓库比较，他使用相同的配置文件，主要的又是是yarn工具比较快。

npm仓库，无论用npm或者yarn, 都是使得Node.js更简单有趣易用的重要组成部分。

### 表现

一段时间后Java和JavaScript都被指责变得慢了。

两个都是从国一个虚拟机把源代码转成字节码执行。虚拟机将经常深度编译字节码为本地代码，通过使用一些列优化技术。

Java和JavaScript有巨大激励去运行更快。在Java和Node.js激励快速的服务端代码。在浏览器JavaScript激励是比客户端程序员表现好 -- 看下一章富网络应用的。

Sun/Oracle Jdk使用的HotSpot, 是一个有多个字节码编译策略的超级虚拟机。名字来自于识别出频繁执行的代码优化更多的代码执行。HotSpot 高优化生产出非常快速的代码。

在JavaScript，我们过去常想象，我们怎么样才能让运行在浏览器的JavaScript实现各类复杂的应用？一个办公文档处理套件在浏览器基于JavaScript处理是不可能的？今天，当然，在蛋糕证明了能吃。我用Google Docs 来编写这个文章，表现很不错。 浏览器端JavaScript 每年进展神速。

Node.js直接受益于刚开始使用谷歌的V8引擎。

一个例子是PeterMarshall，V8增强主工程师， 特别分配一些使用Node.js平台的工作。他描述为什么V8选择从衢州虚拟机到涡轮发动机虚拟机。

机器学习是一个涉及大量使用R、Python进行数据科学计算的数学计算领域。一些领域包括机器学习都基于快速数字计算机。很多原因，JavaScript 弱于此，但是开发一个JavaScript数值计算标准库正在进行中。

另一个视频演示在JavaScript里使用TensorFlow， 使用一个新库：TensorFlow.js。 有和python相同的TensorFlow API， 能够导入预先训练的模型。能够用来分析视频识别训练的对象，同时完全运行在浏览器。

另一场讨论中，IBM的Chris Bailey 讨论了Node.js的性能和可伸缩性问题，特别是在Docker和Kubernetes部署方面。他用一些指标显示Node.js比SpringBoot在吞吐量上有更注目的表现，在应用启动时间和内存占用上。更深的，Node.js 随着V8的改进，性能释放更显著改善。

在那个视频里，Bailey说不应该用Node.js跑计算代码。理解理由是很重要的。因为单线程模型长时间跑计算将会阻塞事件执行。在我的Node.js Web开发书里，我讲了这个问题，并展示了三个处理方法：

+ 算法重构 ---  识别出算法的慢的部分，重构变得更快速
+ 通过事件分发分割大块的计算，所以Node.js 就会定期返回执行线程
+ 把计算移到后台计算

如果JavaScript提高不够显著，仍有两种方法直接集成本地方法到Node.js。最直接的方法是一个本地方法的Node.js模块。Node.js工具链包含  node-gyp 用来处理连接到本地代码模块。这个视频展示一个Rust 库集成到Node.js

<iframe width="560" height="315" src="https://youtu.be/Pfbw4YPrwf4" frameborder="0" allowfullscreen></iframe>

WebAssembly 提供了把其他语言编译到JavaScript子集合的能力让运行更快。
 WebAssembly是一个便携式规范对那些运行在JavaScript引擎的代码。这个视频是一个很好的例子，示范了使用WebAssembly 在Node.js运行代码。
 
 
 ### 富网络引用（RIA）
 十年前软件工业忙于用JavaScript快速引擎实现富网络应用，但是桌面应用觉得无关痛痒。
 
 故事真正开始于超过20年前。Sun和网景公司发布Java Applet 在Netscapte导航条。JavaScript那时作为Java Applets的脚本语言。期望Java Servlet在服务端，Java Applet统治客户端，让我们两端都使用相同的编程语言。但是因为各种原因没有实现。
 
 十年前 JavaScript 开始变得很强大足以自行实现复杂应用。此后RIA 被期望取代Java作为客户端平台。
 
 今天我们看RIA理念的实现。在服务器上用Node.js，JavaScript在客户端，我们进入了天堂。
 
 一些例子：
 
 + Google Docs （用来写本篇文章的地方）像一个典型的办公软件套件，但是运行在浏览器。
 + 强大的框架像 React,Angular, Vue.js，使用Html/CSS作为样式，快速开发基于浏览器的应用。
 + Electron 是Node.js和Chromium Web浏览器支持的跨平台桌面应用开发的混合物。它非常优秀，几个受欢迎的应用像Visual Studio Code ， Atom， GitKraken 和Postman都是用Electron开发的，他们都是非常优秀。
 + 因为Electron/NW.js 使用浏览器引擎，框架类像React/Angular/Vue 被用来做桌面应用。例子看这里 https://blog.sourcerer.io/creating-a-markdown-editor-previewer-in-electron-and-vue-js-32a084e7b8fe
 
 
 Java 作为桌面应用平台并没有因为JavaScript的RIA而消亡。它可能因为Sun 微系统对客户端技术的轻视而消亡。Sun 聚焦于企业客户要求更快的服务端表现。我在哪里看到了第一现场。真正杀死Applets 是一个发现于很多年前的在Java插件和WEb启动时的一个糟糕的安全问题。那个Bug造成世界范围的警告，使用Java applets 和Web启动的应用能轻易停止。
 
 其他类型的Java桌面应用仍然被开发中，在NetBeans和Eclipse IDE的竞赛中仍在活跃。但是在这一领域工作是不景气的，外部的很多开发者工具很少基于Java开发应用。
 
 JavaFX 是例外。
 
 JavaFX 是10年前，作为Sun对IPhone的解决方案。被用来支持开发基于Java 的在手机端的富图形界面应用，能够单独支持开发业务外的Flash IOS应用。但是没有实现。JavaFX仍在使用，但没大火。
 
 随着React， Vue.js 和相似框架出现，越来越令人兴奋。
 
 在这个方面，JavaScript 和Node.js 赢了。
 
 ### 总结
 
 今天服务端代码有很多选择。不再限制在 “ P 语言” （Perl, PHP, Python）和Java， 因为还有Node.js， Ruby, Haskell, Go, Rust, 等等。所以享受选择困难症吧。
 
 所以在这里为什么Java小伙转向Node.js, 就很清楚了，我用Node.js 写代码感到更自由。Java变得笨重，但是Node.js并没有这类问题。如果我被雇佣再次写Java，我也会做，因为是我需要付出的代价。
 
 每个应用都有自己真正的需求。所以因为一个人喜欢Node.js就一直使用它也不对。必须有专门的理由选择一门语言或框架而不是另一个。例如，我最近做的工作涉及XBRL文档。XBRL库最好的实现是用Python，所以需要学习Python来继续进行那个项目。诚实的评估你的需要然后做出对应选择。



