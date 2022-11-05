## ARTS - Review
## 编写好代码的高级技巧
来自medium: [Advanced Techniques and Ideas for Better Coding Skills](https://medium.com/@maladdinsayed/advanced-techniques-and-ideas-for-better-coding-skills-d632e9f9675)


好的开发者是用他们的代码质量来定义的。在软件工程里，书写好的代码意味着节省了可能投资在测试、更新、扩展或者修复bug的钱。本文里， 我将会给你展示一些真实生活中的例子来帮助你整理你已有的代码来重构它使得更健壮，更模块化。这些技术不仅能帮你重构老代码，也能为你将来的编码中提供好的思路。


### 什么是重构，为什么我们要重构？
重构指的是帮你写整洁代码的技术和步骤。这对其他可能会阅读、扩展、重用你的代码的人很重要，使得他不用大范围的修改。

接下来内容是重构已存在代码使它更好的几个例子。

### 不要用没进行单元测试的代码重构生产代码
我的第一个建议是，不要用没进行足够单元测试的代码去重构已存在的代码。我想原因是很明显的：你将会遇到一些难以修复的坏的功能，因为你无法找到哪个是坏的。所以，如果需要重构，首先测试。确保你重构的部分被测试覆盖到。检查php代码覆盖率分析。

### 从最需要重构的地方开始
看下面图片，这是我在github 上发现的一个关于酒店管理系统的真实的工程。这是一个真正开源的工程，所以闭源的代码可能会更差。
![](https://cdn-images-1.medium.com/max/1600/0*o_Xt7O_1WitOzJx9.png)
你看到，三级被标红了。最深点应该是第一个if条件里面的 if/else 语句内容。通常最深点集中在一个简单逻辑上，这让重构变得简单。

### 让你的方法分成几个小方法或配置 文件/数据库 表 来让方法变短
在这个例子中，我们开一个提出来一个私有方法：
![](https://cdn-images-1.medium.com/max/1600/0*GzntaUY08VZDx_mD.png)

下一个最深点是获取post数据和加载视图那里。看看重构之后的add方法。更干净可读、方便测试。
![](https://cdn-images-1.medium.com/max/1600/0*5rTlqNl6knjMKdBk.png)

### 在if语句中使用{}
很多编程语言支持一个条件时候不用加{}， 一些开发者就因为简单就这么用了，但是它可能造成可读性差，很容易因为一个空行造成问题。看以下例子：

![](https://cdn-images-1.medium.com/max/1600/0*qKPPZhC6bI0tcHy8.png)

### 不要用魔法数字魔法字符串
接下来的例子里，你注意到房间数超过250后，返回一个错误信息。这个例子中250就是一个魔法数字。如果不是你写的，你就很难理解它代表的什么。

![](https://cdn-images-1.medium.com/max/1600/0*of8kepNKAX3zsRp5.png)

重构这个方法，我们要指出这个250代表的是最大房间数。所以我们用变量来代替这个硬编码，这样对其他人就很容易理解了。

![](https://cdn-images-1.medium.com/max/1600/0*5nR9rHxduxPvfUZJ.png)

### 如果不需要，就不要用 else 语句
在这个 availablerooms() 方法中，你发现我们很容易摆脱else 代码块，逻辑依然一样。

![](https://cdn-images-1.medium.com/max/1600/0*EL3AvdzpKT9uibeo.png)

### 使用有意义的方法、变量和测试名字
下面的例子，发现两个方法叫 index() 和 room_m(), 对我来说我不知道它们代表啥。如果它们名字描述的是业务，我们就更好理解。

![](https://cdn-images-1.medium.com/max/1600/0*r1mxi52OtJ5RXw-m.png)

### 最大化的利用你编程语言的特性
很多开发者不最大化利用他们语言的特性。很多特性是让你花很少精力使得你的代码更健壮。
看以下例子如何使用更少代码实现相同的结果仅仅利用了类型提示。

![](https://cdn-images-1.medium.com/max/1600/0*qBo2t1VxVGDLd5gZ.png)
![](https://cdn-images-1.medium.com/max/1600/0*lI9BoaA39baxGFY6.png)


我用几个写出更好代码的技巧来结束本篇文章：

+ 使用新的 数组而不是Array()方法
+ 使用 === 代替 == 除非不需要检查数据类型很重要
+ 给公共方法简短有意义的命名，给私有方法长的名字因为他们有固定范围
+ 使用通用的命名，如实现接口的方法add() 对单个类方法使用描述性命名如 addUser() 或 addDocument()
+ 删除类中无用的方法
+ 用is/has 前缀命名返回布尔类型的方法，例如 isAdmin($user) hasPermission($user)
+ 在类方法和属性上始终使用访问修饰符
+ 小心接口污染: 仅使用用户公开的方法
+ 组织类方法，公共方法在上
+ 始终对类使用单一职责原则


---

### 感言
阅读并遵循《阿里巴巴开发手册》， 里面更详细，指导新代码开发， 并结合 Alibaba Coding Guidelines 工具扫描老代码并进行重构。