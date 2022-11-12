## Tip - 窗口函数 Lead， Lag

近来接触了SQL中的  window function .  窗口函数主要作用在对结果集 跨越操作， 如上一条记录、行号、排序等。

平时使用最多的是 row_numer, 作为行号来处理，

随之的是  rank, dense_rank， 近来有一个需求是比较今天、昨天的差额，就用到了适合临近行的 Lead 和 Lag

window function的结构：

```sql
window_function ( expr ) OVER ( 
  PARTITION BY ... 
  ORDER BY ... 
  frame_clause 
)
```

 

### LEAD, LAG

首先字面意思理解， Lead代表领先的，  LAG慢的，落后的， 都是一个相对概念， 相对的就是 current row. 

在使用时候，比较模糊的是 Lead领先 current row, 是向上领先还是向下领先？ 

1.  jack 23
2. rose 24
3. bob 34
4. jim 14



比如 current row 是 3  bob, 那么 lead（bob）， 是2 rose 还是 jim?  其实和 over函数的排序有关

lead(name) over(order by id asc)   ，那么就是jim.



LAG正好是相反的。