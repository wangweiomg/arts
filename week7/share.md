## ARTS - Share

### 说说一次sql优化过程

很容易在网上找到sql优化要点，在这里记录一次本人经历的一个优化过程。

在工作中需要统计一个信息形成报表，最开始的sql 是这样的：

```
SELECT 
	SUM(a + b - c) amount,
    CASE
	    WHEN day >= 2 AND day <= 8 THEN 'm1_1'
	    WHEN day >= 9 AND day <= 20 THEN 'm1_2'
	    WHEN day >= 21 THEN 'm1_3'
    ELSE 'm1_0'
    END  'day_type', create_date

FROM table a
    GROUP BY day_type, create_date
    
    HAVING
    create_date >= ?
    and create_date<= ?
    and day_type != 'm1_0'

```
这个sql在我们库里执行了10秒，于是就需要优化。

首先这是个单表查询的分组函数，在进行全表扫描分组后，再对结果进行了过滤。很明显这个过程不对，应该对过滤后的结果再进行分组，这样大大减少了无用数据的查询。

修改后是这样的：

```
SELECT 
	SUM(a + b - c) amount,
    CASE
	    WHEN day >= 2 AND day <= 8 THEN 'm1_1'
	    WHEN day >= 9 AND day <= 20 THEN 'm1_2'
	    WHEN day >= 21 THEN 'm1_3'
    ELSE 'm1_0'
    END  'day_type', create_date

FROM table a
WHERE 
	 create_date >= ?
    and create_date<= ?
    
    GROUP BY day_type, create_date
    HAVING
    day_type != 'm1_0'



```

这样就大大提高了响应速度，在2秒内。

继续优化，看到我们的 **HAVING day_type != 'm1_0'** ，这个在sql 里就是 不需要 day<2 的数据，直接在where 里过滤就够了。于是修改后是这样的。

```
SELECT 
	SUM(a + b - c) amount,
    CASE
	    WHEN day >= 2 AND day <= 8 THEN 'm1_1'
	    WHEN day >= 9 AND day <= 20 THEN 'm1_2'
	    WHEN day >= 21 THEN 'm1_3'
    ELSE 'm1_0'
    END  'day_type', create_date

FROM table a
WHERE 
	 create_date >= ?
    and create_date<= ?
    and day >=2
    
    GROUP BY day_type, create_date
    
```

再之后，使用 explain 执行，发现没有建立索引于是就对 create_date 和 day建立索引，最终的结果响应在 1秒左右。
