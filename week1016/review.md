## Reivew
## [SQL 存储过程和游标介绍](https://towardsdatascience.com/introduction-to-procedures-and-cursors-in-sql-f9d9b9ea1fe7)

#### 学习为一个关系型数据库写存储过程和游标。

如果你学习更多关于SQL特别是从数据科学的角度，你可以学习DataCamp的 ["数据科学SQL介绍"](https://www.datacamp.com/courses/intro-to-sql-for-data-science)课程。

SQL对每个现代软件工程师都是必备技能。因为几乎全部软件都依赖几种数据集成的RDBMS（关系型数据库管理系统）。无论是网页应用，无论是API或者内部应用，都会用到关系型数据库。SQL就是查询关系型数据库的语言。

作为一个数据科学家，了解SQL和它关联知识非常重要。为了能够查询一个数据库系统获得你想要的处理的特定问题返回，SQL是最低需求。

在这个最新DataCamp视频中， David Robinson (DataCamp首席数据科学家)为我们展示他这么在一个数据科学问题中使用SQL。请检出它，他的工作流程非常有趣。

在本篇指导中，你将会学到些存储过程和游标；SQL另一个重要面。你是否曾想要你的RDBMS 在某一个特定的时间发生时自动响应一个确定的动作？ 例如，我们说你创建了一个新的员工记录在你叫做Employees 的表中，你想要这个反射到另一个关联表 像 Departments 中。好了，你可以获得正确的教程。

在这个教程中，你将会学到：

* 存储过程在RDBMS里是什么
* 你怎么能写一个存储过程
* 存储过程的不同类型
* 什么是RDBMS的游标
* 怎么写不同类型的游标
* 不同类型的游标

听起来兴奋吧？让我们开始吧。

### 在一个RDBMS里什么是一个存储过程？
在继续存储过程和游标之前，你将会需要知道一点关于PL/SQL ，它是一个块结构语言能够让像你一样的开发者用过程声明来拥有SQL的力量。但是你不会以传统方式学习，会随着你的需要学习它。

所以如果你拥有SQL查询并且你想要重复执行它。存储过程是其中的一个解决方案。存储过程通常在这个上下文中被调用，因为他们保持了存储并在特定操作或一系列动作时被触发。存储过程也被称为 ```Procs```.

现在你将会看到怎么写一个存储过程。

### 书写存储过程：

书写存储过程通用的语法如下：

```
CREATE PROCEDURE procedure_name
AS 
sql_statement
GO;
```

请注意这些语法介乎适用于所有的RDBMS不管是Oracle， 还是PostgreSQL 还是MySQL。

创建存储过程之后，你要执行它。以下是语法。

```
EXEC procedure_name;
```

让我们写一个简单的存储过程。考虑如下快照，是从一个RDBMS 以一个叫Customers 表里取得。

```

+------------+--------------+-------------+---------------+--------+------------+---------+
| CustomerID | CustomerName | ContactName | Address       | City   | PostalCode | Country |
+------------+--------------+-------------+---------------+--------+------------+---------+
|          1 | Jack         | Rose        | Obere Str. 57 | Berlin | 12209      | Germany |
|          2 | Anna         | Carols      | Avda 2222     | London | WA1 1DP    | UK      |
|          3 | Jim          | Mike        | Hemels 23     | Lule   | S-987 22   | Sweden  |
|          4 | Bob          | Allen       | Zhengzhou 123 | mexico | 05021      | Mexico  |
|          5 | Catalina     | Jerry       | Tomcat 786    | London | WA1 2XP    | UK      |
+------------+--------------+-------------+---------------+--------+------------+---------+

```

你将会写一个叫做 ```SelectAllCustomers```的存储过程，将会从```Customers```表中查出所有的客户。

```
CREATE PROCEDURE SelectAllCustomers
AS 
SELECT * FROM Customers
GO;

```

(译者注) 注意：
如果使用的MySQL，声明是如下的：

```
CREATE PROCEDURE SelectAllCustomers()
  BEGIN
  SELECT * FROM Customers;
  END;

```


执行 ```SelectAllCustomers``` 通过：

```
Exec SelectAllCustomers;
```


(译者注) 注意：
如果是MySQL，执行是如下的：

```
CALL SelectAllCustomers;
```

存储过程能够被是独立的语句块，这使得他们能够独立于任何表，而不是像之前一个表。以下的例子是创建一个简单的存储过程展示 ‘Hello World!’作为输出当执行后。

```
CREATE PROCEDURE welcome
AS 
BEGIN
dbms_output.put_line('Hello World!');
END;
```

(译者注)注意：
在MySQL中写法是：

```
CREATE PROCEDURE welcome()
  BEGIN
  SELECT 'Hello World!';
  END;

CALL welcome;
```

有两种方法执行一个独立的过程。

* 使用 EXEC关键字
* 从 PL/SQL 块里调用过程名

以上过程叫做‘welcome’能被 EXEC关键字调用：

```
EXEC welcome;
```

你将会看到下个方法在一个PL/SQL块中调用另一个一个过程.

```
BEGIN
welcome;
END;
```

删除一个储存过的过程不是一个大问题：

```
DROP PROCEDURE procerure-name;
```

过程也能够基于参数。可以传单个参数也可以多个。现在你将会学这种。

你会使用同样的Customers表。

你将会写一个存储过程查询Customers 从一个特有的城市：

```
CREATE PROCEDURE SelectAllCustomers @City nvarchar(30)
AS
SELECT * FROM Customers WHERE City = @City
GO;
```

我们剖析共同的原则：
* 你写了第一个 @City 定义了它的类型和长度作为将会在执行时传给过程的参数之一。
* 第二个 @City 被指定给局部变量 Customers表中的 City  列。

过程执行：

```
EXEc SelectAllCustomers City = "London";
```

(译者注) 注意，MySQL中写法是：

```
CREATE PROCEDURE SelectAllCustomers(zCity VARCHAR(30))
  BEGIN
    SELECT * FROM Customers WHERE City = zCity;
  END;


CALL SelectAllCustomers('London');

// 此处要注意的是，如果之前创建了SelectAllCustomers 过程，要先删除。 变量名不要和表列名同名。
```



我们再来看另外一种。

多个参数的存储过程和之前的很像。你只需要拼接他么就行。

```
CREATE PROCEDURE SelectAllCustomers @City nvarchar(30), @PostalCode nvarchar(10)
AS 
SELECt * FROM Customers Where City = @City AND PostalCode = @PostalCode
Go;

```

执行过程：

```
EXEC SelectAllCustomers City = "London", PostalCode = "WA1 1DP";
```

(译者注)注意：
MySQL中写法是：

```
CREATE PROCEDURE SelectAllCustomers2(zCity VARCHAR(30), zPostalCode VARCHAR(30))
  BEGIN
    SELECT * FROM Customers WHERE City = zCity AND PostalCode = zPostalCode;

  end;

CALL SelectAllCustomers2('London', 'WA1 1DP');
```

是不是上面的代码不是很易读吗？ 当代码可读，会有更多有趣的事。这就是过程的全部。你现在将要学游标。

### RDBMS的游标是什么？

数据库像Oracle 创建了一个内存区域，被称为 context area , 对于处理一个SQL语句，包含所有需要处理的语句信息，例如 -- 行数。

一个游标是一个上下文区域的指针。PL/SQL 通过游标控制上下文区域。一个游标是一个SQL语句执行时系统内存创建的一个临时工作区域。一个游标包含一个查询语句的信息和它操作的数据行的信息。所以，游标被用来在一个大的数据库中加快处理查询速度。你需要用数据游标的理由是你需要个别行性能表现。

游标可以是两种类型：

* 隐式游标
* 显式游标

现在将看到这么写不同类型的游标。

### 书写游标

你将会通过理解什么是隐式游标来开始这个章节。

**隐式游标**是当一个SQL语句执行时没有显式游标定义时，ORACLE会自动创建一个隐式游标。程序员不能控制隐式游标和它里面的信息。当一个DML（Data Manipulation Language 数据操纵语言）语句(INSERT, UPDATE, DELETE)发出， 一个隐式游标就被关联到这条语句。对于 INSERT 操作，游标持有需要被插入的数据。对于UPDATE 和 DELETE操作，游标鉴定将被影响的行。

你可以将最新的隐式游标成为SQL游标，它始终具有以下属性：

* %FOUND
* %ISOPEN
* %NOTFOUND
* %ROWCOUNT

以下图片描述这几个属性的含义：

属性 | 描述
---|---
%FOUND | 返回TRUE， 如果一个INSERT ，UPDATE或DELETE语句影响一个或多行， 后者一个SELECT INTO 语句返回一行或多行。其他的，返回FALSE
%NOTFOUND | 和%FOUND逻辑相反。当一个INSERT ，UPDATE，或DELETE语句没有影响到行数据，或者SELECT INTO 语句无数据返回，返回TRUE。否则返回FALSE。
%ISOPEN | 对隐式游标总是返回FALSE，因为ORACLE在执行完SQL语句后会自动关闭SQL游标。
%ROWCOUNT | 返回被INSERT、UPDATE、DELETE影响的行数，或者被SELECT INTO 语句影响返回的行数。


我们看一个来自于叫 Employee表的数据快照

```
+----+----------+------+---------+---------+
| ID | NAME     | AGE  | ADDRESS | SALARY  |
+----+----------+------+---------+---------+
|  1 | Michael  |   32 | US      | 6500.00 |
|  2 | John     |   26 | CANADA  | 8000.00 |
|  3 | David    |   33 | ITALY   | 9500.00 |
|  4 | Mitchell |   42 | GERMANY | 4500.00 |
|  5 | Robin    |   29 | US      | 6000.00 |
+----+----------+------+---------+---------+

```

现在你将会写一个游标对小于30岁的人没人增加1000元薪水。

```
DECLARE
total_rows number(2);
BEGIN
UPDATE Employee
SET salary = salary + 1000
where age < 30;
IF sql%notfound THEN
    dbms_output.put_line('No employees found for under 30 age');
ELSIF sql%found THEN
    total_rows := sql%rowcount;
    dbms_output.put_line( total_rows || ' employees updated ');
END IF;
END;

```
我们回顾下你写的：

* 你定义了一个叫 ```total_rows```的变量来存储 employees 被游标改变的的数量。
* 你用 BEGIN  启动了 游标块写了一个简单SQL查询更新年龄小于30的薪水。
* 你处理了输出年龄小于30的员工的数据直到没有数据。你使用 ```%notfound```属性。注意隐式游标 sql 来存储的所有对应的信息。
* 最后，你用游标的 ```%rowcount```属性打印出了受影响的行数。

很棒！干的不错。
当以上代码执行在SQL里，他铲射是哪个如下结果：
> 2 Employees updated (assume there are 2 records where age < 30)
> 

现在学习显式游标。

显式游标给了在上下文区域更多的定义控制。创建在一个返回多行的SELECT查询语句之上。

创建一个显式游标的语法是：

```
CURSOR cursor_name IS  select_statement;
```

如果你工作中使用显式游标，你需要跟着一些列的步骤如下：

* 在内存里声明游标初始化
* 打开游标分配一块内存区域
* 拉去游标回收数据
* 关闭游标释放内存

一下图片表示一个典型的显式内存生命周期：

![](https://cdn-images-1.medium.com/max/1600/0*rTDQzZNikWs2Eg-O)

你将会学习更多关于这些。

### 声明游标
你声明一个游标用SELECT语句，例如：

```
CURSOR C IS SELECT id, name, address FROM Employee where age > 30;

-- mysql 写法：
DECLARE cur1 CURSOR FOR SELECT NAME, AGE FROM Employee;
```


### 打开一个游标
当你打开游标，CPU 为游标分配内存做好准备为获取SQL语句返回的行。例如，我们将会打开上面定义的游标如下：

```
OPEN  C;
```

### 抓取游标

抓取游标获取一次操作一行从关联的表中，在SQL中意味着游标。

```
FETCH C INTO C_id, C_name, C_address;
```

### 关闭游标

关闭游标意味着释放分配的内存。你将会关闭上面打开的游标：

```
CLOSE C;
```

现在你将会吧这些片用一个有意义的方式放在一起：

### 合并这些步骤

```
DECLARE
C_id Employee.ID%type;
C_name Employee.NAME%type;
C_address Employee.ADDRESS%type;
CURSOR C is
SELECT id, name, address FROM Employee where age > 30;
BEGIN
OPEN C;
LOOP
FETCH C INTO C_id, C_name, C_address; 
dbms_output.put_line(ID || ' ' || NAME || ' ' || ADDRESS); 
EXIT WHEN C%notfound;
END LOOP;
CLOSE C;
END;
```

注意，在MySQL中是：

```
CREATE PROCEDURE curdemo()
BEGIN
  DECLARE done INT DEFAULT FALSE;
  DECLARE a CHAR(16);
  DECLARE b, c INT;
  DECLARE cur1 CURSOR FOR SELECT id,data FROM test.t1;
  DECLARE cur2 CURSOR FOR SELECT i FROM test.t2;
  DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = TRUE;

  OPEN cur1;
  OPEN cur2;

  read_loop: LOOP
    FETCH cur1 INTO a, b;
    FETCH cur2 INTO c;
    IF done THEN
      LEAVE read_loop;
    END IF;
    IF b < c THEN
      INSERT INTO test.t3 VALUES (a,b);
    ELSE
      INSERT INTO test.t3 VALUES (a,c);
    END IF;
  END LOOP;

  CLOSE cur1;
  CLOSE cur2;
END;
```

你也学到了声明游标变量 C_id, C_name, 和 C_address。 ```C_id Employee.ID%type```； 这个确保C_id 创建时候和在Employee表中的ID是相同的数据类型。

通过使用 LOOP 你循环游标的获取记录展示它。你也可以处理游标没有找到记录的情况。

### 总结：
恭喜，你学完了。你学到了数据库中两个最流行的主题--存储过程和游标。这在应用中处理巨量事务时候是非常普遍的。你猜对了。银行在太古时间都用上了这些。你学会了怎么写一个存储过程，他们是不同的类型，为什么会是这样。你也学到了游标和他的几个写法。

以下是是引用：















