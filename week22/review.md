## ARTS - Review  补12.3
* gallery n. 画廊、走廊，
* vast amount of 大量的
* standalone utilities 独立应用程序
* relational adj. 亲属的，有关系的
* governing v. 统治，控制
* enforces v.强制执行
* inconsistent adj. 不一致的
* orphan adj. 孤立的
* embed 嵌入
* commercial 商业的
* dedicate 奉献
* demanding 苛求的
* consists 包括
* pronounce 发音

1.3.1 What is MySQL?

MySQL, the most popular Open Source SQL database management system, is developed, distributed, and supported by Oracle Corporation.

The MySQL website (http://www.mysql.com/) provides the latest information about MySQL software.

> 什么是mysql, 最流行的开源SQL数据库挂历系统，由甲骨文集团开发、发布和支持。MySQL的网址 http://www.mysql.com ，提供最新的MySQL软件信息。


MySQL is a database management system.
> MySQL是一个数据库管理系统

A database is a structured collection of data. It may be anything from a simple shopping list to a picture gallery or the vast amounts of information in a corporate network. To add, access, and process data stored in a computer database, you need a database management system such as MySQL Server. Since computers are very good at handling large amounts of data, database management systems play a central role in computing, as standalone utilities, or as parts of other applications.
> 一个数据库是一个结构化的数据集合。他可以是一个简单的图片画廊的购物清单，或者是一个集团网络的大量信息。去添加、操作，处理保存在计算机中的数据库，你需要一个数据库管理系统例如MySQL 服务器。自从电脑开始非常擅长处理大量数据，数据库管理系统就在计算、独立应用，应用的一部分中扮演了一个中心角色。
>




MySQL databases are relational.
> MySQL数据库是关系型的。

A relational database stores data in separate tables rather than putting all the data in one big storeroom. The database structures are organized into physical files optimized for speed. The logical model, with objects such as databases, tables, views, rows, and columns, offers a flexible programming environment. You set up rules governing the relationships between different data fields, such as one-to-one, one-to-many, unique, required or optional, and “pointers” between different tables. The database enforces these rules, so that with a well-designed database, your application never sees inconsistent, duplicate, orphan, out-of-date, or missing data.
> 一个关系型的数据库存储数据在独立的表中而不是把全部的数据放在一个大的储存室。数据库结构被优雅组织在物理文件中方便查询。逻辑模型，像数据库对象，表，视图，行，列，提供一个复杂的程序环境。你设置规则控制不同数据属性的关系，像一对一，一对多，唯一，必须或非必须，还有两个不同表之间的”指针“。数据库强制执行这些规则，所以就会产生一个良好设计的数据库，你的应用就不会看到不一致、重复、孤立、过期的或丢失数据。

The SQL part of “MySQL” stands for “Structured Query Language”. SQL is the most common standardized language used to access databases. Depending on your programming environment, you might enter SQL directly (for example, to generate reports), embed SQL statements into code written in another language, or use a language-specific API that hides the SQL syntax.

> SQL 作为MySQL一部分代表 ”结构化查询语言“。 SQL是最常见的操作数据库的标准语言。决定于你的程序环境，你可能直接进入SQL(例如，构造报表), 把sql语句嵌入到另外一种语言里，或者用一个隐藏SQL语法的特定语言api。

SQL is defined by the ANSI/ISO SQL Standard. The SQL standard has been evolving since 1986 and several versions exist. In this manual, “SQL-92” refers to the standard released in 1992, “SQL:1999” refers to the standard released in 1999, and “SQL:2003” refers to the current version of the standard. We use the phrase “the SQL standard” to mean the current version of the SQL Standard at any time.
> SQL由 ANSI/ISO SQL标准定义。SQL定义从1986进化了几个版本。在本手册，”SQL-92”表示在1992年发布的SQL-92“标准，”SQL:1999“表示1999年发布的。SQL：2003表示当前发布的版本。我们使用词语“SQL标准”意思是任何时间的当前版本的标准版本。

MySQL software is Open Source.

Open Source means that it is possible for anyone to use and modify the software. Anybody can download the MySQL software from the Internet and use it without paying anything. If you wish, you may study the source code and change it to suit your needs. The MySQL software uses the GPL (GNU General Public License), http://www.fsf.org/licenses/, to define what you may and may not do with the software in different situations. If you feel uncomfortable with the GPL or need to embed MySQL code into a commercial application, you can buy a commercially licensed version from us. See the MySQL Licensing Overview for more information (http://www.mysql.com/company/legal/licensing/).
> Mysql 软件是开源的。
> 开源意味着可以被任何人使用修改的软件。任何人可以从网上下载MySQL软件使用它而不付费。如果你希望，你可以学习源码，修改它适应你的需要。MySQL软件使用GPL授权协议，http://www.fsf.org/licenses/， 定义了不同场景下可以和不可以做的事。如果你感到不舒服对GPL或者需要嵌入MySQL代码到一个商业应用里，你看购买一个商业授权版本从us. 

The MySQL Database Server is very fast, reliable, scalable, and easy to use.

If that is what you are looking for, you should give it a try. MySQL Server can run comfortably on a desktop or laptop, alongside your other applications, web servers, and so on, requiring little or no attention. If you dedicate an entire machine to MySQL, you can adjust the settings to take advantage of all the memory, CPU power, and I/O capacity available. MySQL can also scale up to clusters of machines, networked together.

> Mysql 数据库非常快，可靠，可扩展，易于使用。
> 如果那就是你要找的，你应该尝试一下它。MySQL服务器可以舒适的运行在一个台式机或笔记本，或你的其他应用的一边，web服务器，等。需要很少或不需要注意。如果你奉献所有的机器资源给MySQL，你可以调整设置来使得内存、CPU和IO容量物尽其用。MySQL也可以机器集群，网络连接到一起。

MySQL Server was originally developed to handle large databases much faster than existing solutions and has been successfully used in highly demanding production environments for several years. Although under constant development, MySQL Server today offers a rich and useful set of functions. Its connectivity, speed, and security make MySQL Server highly suited for accessing databases on the Internet.
> MySQL服务原来开发出来去处理大数据比现存解决方案都快得多，已经成功用在高苛求的生产环境好多年了。尽管在常规开发底下，MySQL服务今天提供了丰富的有用的方案。它的连通性，速度和安全性使得MySQL服务非常适合处理网上数据。

MySQL Server works in client/server or embedded systems.

The MySQL Database Software is a client/server system that consists of a multithreaded SQL server that supports different back ends, several different client programs and libraries, administrative tools, and a wide range of application programming interfaces (APIs).
>  MySQL工作在客户端/服务端 或者嵌入式系统。
> MySQL数据库软件是一个 客户端、服务端系统，包括多线程SQL服务器支持不同的后台，几个不同的客户端程序和库，管理员工具，宽范围的应用程序接口。


We also provide MySQL Server as an embedded multithreaded library that you can link into your application to get a smaller, faster, easier-to-manage standalone product.

> 我们也提供MySQL服务组委一个嵌入式多线程库，你可以连接进入你的应用获得一个更小，更快，更方便管理的独立产品。

A large amount of contributed MySQL software is available.

MySQL Server has a practical set of features developed in close cooperation with our users. It is very likely that your favorite application or language supports the MySQL Database Server.

The official way to pronounce “MySQL” is “My Ess Que Ell” (not “my sequel”), but we do not mind if you pronounce it as “my sequel” or in some other localized way.

> 大量的捐助MySQL软件是有用的。
> MySQL服务有一套实用的功能是封闭的集团用户开发的。它非常娘你瞎换应用或语言支持MySQL数据库服务。
> 专业方式发音 MySQL是 “My Ess Que Ell” (不是 my sequel), 但是我们不介意你的发音是“my sequel”或者其他的个性化叫法。
