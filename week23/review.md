## ARTS - Review 补12.10
## 1.3.2 MySQL重要特性

* respects n. 敬意；考虑；关联
* roadmap n. 路标
* appropriate adj. 适合的
* Nutshell n. 果壳，简言之，一言以蔽之
* Internals  内部
* Portability 可移植性
* commercial adj. 商用的
* Internals 内幕
* Portability 可移植性
* temporary 临时的
* optimized 优化
* retrieve v. 取回
* restriction 限制
* privilege 特权
* flexible 灵活
* Scalability n. 可扩展性
* consist of  由...组成
* Connectivity 连通性
* Localization 局部化，地方化
* specified 规定



> This section describes some of the important characteristics of the MySQL Database Software. In most respects, the roadmap applies to all versions of MySQL. For information about features as they are introduced into MySQL on a series-specific basis, see the “In a Nutshell” section of the appropriate  
Manual:
> 

这一章节讲述MySQL数据库软件的一些重要特性。在大多数方面，路线图适合MySQL所有版本。有关基于系列的MySQL特性介绍，看相对应的 “简言” 手册。


 **Internals and Portability**
> 
> Written in C and C++.

> Tested with a broad range of different compilers.

> Works on many different platforms. See https://www.mysql.com/support/supportedplatforms/database.html.

> For portability, uses CMake in MySQL 5.5 and up. Previous series use GNU Automake, Autoconf, and Libtool.

> Tested with Purify (a commercial memory leakage detector) as well as with Valgrind, a GPL tool (http://developer.kde.org/~sewardj/).

> Uses multi-layered server design with independent modules.

> Designed to be fully multithreaded using kernel threads, to easily use multiple CPUs if they are available.

> Provides transactional and nontransactional storage engines.

> Uses very fast B-tree disk tables (MyISAM) with index compression.

> Designed to make it relatively easy to add other storage engines. This is useful if you want to provide an SQL interface for an in-house database.

> Uses a very fast thread-based memory allocation system.

> Executes very fast joins using an optimized nested-loop join.

> Implements in-memory hash tables, which are used as temporary tables.

> Implements SQL functions using a highly optimized class library that should be as fast as possible. Usually there is no memory allocation at all after query initialization.

> Provides the server as a separate program for use in a client/server networked environment, and as a library that can be embedded (linked) into standalone  applications. Such applications can be used in isolation or in environments where no network is available.

内部和可移植性

* 用C和C++写的。
* 用多种不同的编译器测试
* 在多个不同的平台工作。
* 为了可移植性，使用CMake 在MySQL5.5及以上。之前的系列使用GNU automake，autoconf,和libtool.
* 使用 purify(一款商用内存泄露检测器) 测试 ，同时还有 Valgrind, 一个GPL工具。
* 使用具有独立模块的多层服务器设计。
* 设计成使用内核线程完全多线程的，方便使用多核CPU。
* 提供事务支持和非事务支持策略引擎。
* 使用非常快的索引压缩的B-树硬盘表(MyISAM)
* 设计成相对易于添加其他策略引擎。这很有用如果你想为一个内部数据库提供一个SQL接口。
* 使用一个非常快速的基于线程的内存分配系统。
* 使用乐观自旋锁执行非常快
* 实现基于内存的哈希表，被用来作为临时表
* 实现SQL方法用一个高度优化的尽可能快的类库。通常在查询初始化后根本没有内存分配。
* 用客户端/服务端网络环境提供作为一个隔离的程序服务，作为可以被单个应用连接的类库。这些应用可以用事务隔离或者无网可用。




>Data Types

>Many data types: signed/unsigned integers 1, 2, 3, 4, and 8 bytes long, FLOAT, DOUBLE, CHAR, VARCHAR, BINARY, VARBINARY, TEXT, BLOB, DATE, TIME, DATETIME, TIMESTAMP, YEAR, SET, ENUM, and OpenGIS spatial types. See Chapter 11, Data Types.

**数据类型**

很多数据类型： 整型 1，2，3，4， 8字节long, FLOAT , DOUBLE, CHAR, VARCHAR,BINARY,VARBINARY,TEXT,BLOB,DATE,TIME,DATETIME,TIMESTAMP,YEAR,SET,ENUM,GIS特殊类型你给，看第11章节，数据类型。

> Fixed-length and variable-length string types.
定长和不定长 string类型

> Statements and Functions

> Full operator and function support in the SELECT list and WHERE clause of queries. For example:

>mysql> SELECT CONCAT(first_name, ' ', last_name)
    -> FROM citizen
    -> WHERE income/dependents > 10000 AND age > 30;
语句与方法。

全部操作和方法支持在 SELECT 列表和 WHERE 语句查询里，例如：
...

>    
Full support for SQL GROUP BY and ORDER BY clauses. Support for group functions (COUNT(), AVG(), STD(), SUM(), MAX(), MIN(), and GROUP_CONCAT()).


>Support for LEFT OUTER JOIN and RIGHT OUTER JOIN with both standard SQL and ODBC syntax.

>Support for aliases on tables and columns as required by standard SQL.

>Support for DELETE, INSERT, REPLACE, and UPDATE to return the number of rows that were changed (affected), or to return the number of rows matched instead by setting a flag when connecting to the server.

>Support for MySQL-specific SHOW statements that retrieve information about databases, storage engines, tables, and indexes. Support for the INFORMATION_SCHEMA database, implemented according to standard SQL.
>

* 完全支持SQL分组和排序。支持分组函数(COUNT(), AVG(), STD(), SUM(), MAX(), MIN(),和GROUP_CONCAT())
* 支持表、列别名，这是SQL标准必须的
* 支持 DELETE,INSERT,REPLACE, 和UPDATE返回改变（命中）的行数，或者连接服务器时候通过设置一个标志来返回匹配的行数。
* 支持MySQL特有的 SHOW 语句，返回数据库、存储引擎、表、索引信息。支持INFORMATION_SCHEMA数据库，根据标准SQL实现。


> An EXPLAIN statement to show how the optimizer resolves a query.


> Independence of function names from table or column names. For example, ABS is a valid column name. The only restriction is that for a function call, no spaces are permitted between the function name and the “(” that follows it. See Section 9.3, “Keywords and Reserved Words”.

> You can refer to tables from different databases in the same statement.

一个EXPLAIN语句显示一个查询的优化解决方案。

取决于表、列名中的方法名。例如，ABS 是一个验证的列名。仅有的限制是对于一次方法调用，在方法名和"("之间不允许有空格的。


> Security

>A privilege and password system that is very flexible and secure, and that enables host-based verification.

>Password security by encryption of all password traffic when you connect to a server.
>

安全

一个特权和密码系统是非常灵活安全，可以基于主机验证。

密码安全通过加密所有密码加护当连接到一个服务器。





> Scalability and Limits

> Support for large databases. We use MySQL Server with databases that contain 50 million records. We also know of users who use MySQL Server with 200,000 tables and about 5,000,000,000 rows.

> Support for up to 64 indexes per table. Each index may consist of 1 to 16 columns or parts of columns. The maximum index width for InnoDB tables is either 767 bytes or 3072 bytes. See Section 15.6.1.6, “Limits on InnoDB Tables”. The maximum index width for MyISAM tables is 1000 bytes. See Section 16.2, “The MyISAM Storage Engine”. An index may use a prefix of a column for CHAR, VARCHAR, BLOB, or TEXT column types.
> 

可扩展性和限制

支持大数据量。我们使用包含五千万记录的MySQL 服务器。我们也了解使用20万个表和大约50亿行数据的MySQL服务器用户。

支持每个表最高64个索引。每个索引可能由1到16列或部分列组成。InnoDB 表的最大索引是767字节或3072字节。MyISAM表的最大索引宽度是1000字节。一个索引可能使用一个列前缀，对于CHAR,VARCHAR,BLOB,或TEXT列类型。






> **Connectivity**

> Clients can connect to MySQL Server using several protocols:

> Clients can connect using TCP/IP sockets on any platform.

连通性

客户端可以使用几种协议连接到MySQL 服务器。

客户端可以使用使用 TCP/IP 套接字在任何平台。


> On Windows systems, clients can connect using named pipes if the server is started with the --enable-named-pipe option. Windows servers also support shared-memory connections if started with the --shared-memory option. Clients can connect through shared memory by using the --protocol=memory option.

> On Unix systems, clients can connect using Unix domain socket files.

在windows系统，客户端可以使用名为管道来连接，使用 --enabled-named-pipe 选项。windows服务也支持共享内存连接，如果使用 --shared-memory 选项。客户端可以使用 --protocaol=memory选项来共享内存连接。

在Unix系统，客户端可以使用unix主机socket文件连接。


> MySQL client programs can be written in many languages. A client library written in C is available for clients written in C or C++, or for any language that provides C bindings.

> APIs for C, C++, Eiffel, Java, Perl, PHP, Python, Ruby, and Tcl are available, enabling MySQL clients to be written in many languages. See Chapter 28, Connectors and APIs.

> The Connector/ODBC (MyODBC) interface provides MySQL support for client programs that use ODBC (Open Database Connectivity) connections. For example, you can use MS Access to connect to your MySQL server. Clients can be run on Windows or Unix. Connector/ODBC source is available. All ODBC 2.5 functions are supported, as are many others. See MySQL Connector/ODBC Developer Guide.

> The Connector/J interface provides MySQL support for Java client programs that use JDBC connections. Clients can be run on Windows or Unix. Connector/J source is available. See MySQL Connector/J 5.1 Developer Guide.

> MySQL Connector/NET enables developers to easily create .NET applications that require secure, high-performance data connectivity with MySQL. It implements the required ADO.NET interfaces and integrates into ADO.NET aware tools. Developers can build applications using their choice of .NET languages. MySQL Connector/NET is a fully managed ADO.NET driver written in 100% pure C#. See MySQL Connector/NET Developer Guide.
> 

Mysql 客户端编程可以使用很多语言。一个用C写的客户端库，对那些任何提供和C绑定的语言写的客户端都是可用的。

API对 C,C++,Eiffel... ，都是可用的。

连接器/ODBC 接口提供MySQL支持用ODBC连接程序的能力。例如，你可以使用MS Access 连接你的MySQL服务。客户端可以运行在windows或unix. 连接器/ODBC 资源是可用的。所有ODBC 2.5 方法和其他的一样，都被支持。

连接器/J 接口提供MySQL支持Java客户端程序使用JDBC连接的能力。客户段可以运行在windows或Unix.连接器/J 资源是可用的。

MySQL 连接器/NET 让程序员轻松创建 .NET 安全，高性能数据连接的MySQL应用程序。它实现了必要的AD0.NET 接口和集成进AD0.NET 软件工具里。开发者使用它们的.NET语言作为构建应用的选择。
MySQL 连接器/NET 是一个完全ADO.NET 驱动管理的100% C#写的。

> Localization

> The server can provide error messages to clients in many languages. See Section 10.11, “Setting the Error Message Language”.

> Full support for several different character sets, including latin1 (cp1252), german, big5, ujis, several Unicode character sets, and more. For example, the Scandinavian characters “å”, “ä” and “ö” are permitted in table and column names.

> All data is saved in the chosen character set.

> Sorting and comparisons are done according to the default character set and collation. is possible to change this when the MySQL server is started (see Section 10.3.2, “Server Character Set and Collation”). To see an example of very advanced sorting, look at the Czech sorting code. MySQL Server supports many different character sets that can be specified at compile time and runtime.

> The server time zone can be changed dynamically, and individual clients can specify their own time zone. See Section 5.1.13, “MySQL Server Time Zone Support”.

本地化

服务可以提供多重语言的错误信息。

对集中不同字符集全支持，包括 latin1, german, big5, ujis, 集中unicode字符集，和更多其他。例如， 斯堪的纳维亚语 在表、列名是允许的。

所有数据用选中的字符集保存。

排序和对比根据默认字符集和规则已经完成了，当启动服务器后也可以改变这些。看一个非常先进的排序案例，看捷克排序代码。MySQL支持多种字符集，可以在编译器和运行期规定。

服务器时区可以动态修改，初始化客户端可以对顶他们自己的时区。



> Clients and Tools

> MySQL includes several client and utility programs. These include both command-line programs such as mysqldump and mysqladmin, and graphical programs such as MySQL Workbench.

> MySQL Server has built-in support for SQL statements to check, optimize, and repair tables. These statements are available from the command line through the mysqlcheck client. MySQL also includes myisamchk, a very fast command-line utility for performing these operations on MyISAM tables. See Chapter 4, MySQL Programs.

> MySQL programs can be invoked with the --help or -? option to obtain online assistance.
> 

客户端和工具

MySQL包含集中客户端段和工具编程。这些包括 命令行编程像 mysqldump 和mysqladmin,和图形化编程像 mysql workbench.

MySQL服务器被设计成支持SQL语句检查，优化修复表。这些语句通过mysqlcheck 客户端连接命令行。MySQL也包括myisamchk， 一个非常快速的命令行工具来操作MyISAM表。

MySQL程序可以用 --help 或 -? 选项来查阅。