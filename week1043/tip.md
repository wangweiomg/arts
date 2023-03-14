## ARTS - Tip -  一次mybatis 与 java8 LocalDateTime的问题

### 问题描述

之前使用Java model ,mybatis 来操作mysql数据库， 对于mysql datetime类型一直使用的是 ```java.util.Date``` ， java8 提供了java.time.LocalDateTime 新的日期解决方案， 所以就引入了项目，然后在批量更新中用到，出现了无法更新的问题。

具体报错:  SQL语法错误



### 环境与版本

```xml
mybatis: 3.5.1
mysql: 5.7
mysql driver: 8.0.26

mybatis xml写法是：

<update id="batchUpdate" parameterType="xxx">
        <foreach collection="list" item="item" index="index" separator=";">
            UPDATE t_table
            SET
                complete_date = #{item.completeDate}
                
            </set>
             WHERE id = #{item.id}
        </foreach>

</update>
```

### 解决过程

首先是自认为 mybatis 解析 LocalDateTime出现了问题，所以搜索关键字是 " mybatis update localdatetime " ， 在 [Java 8 LocalDate mapping with mybatis](https://stackoverflow.com/questions/25113579/java-8-localdate-mapping-with-mybatis)  的回答说是要 增加 mybatis-typehandlers-jsr310 来处理 typeHandler 的问题， 但是在

[LocalDate Cause TypeException · Issue #1549 · mybatis/mybatis-3 (github.com)](https://github.com/mybatis/mybatis-3/issues/1549)  回答中说到，![image-20230314095235220](http://qiniu.honeywen.com/img/20230314095237.png)

mybatis 3.5.1已经解决了这个问题，不需要重新更新 。 所以怀疑是其他问题， 换关键字 搜索   mybatis batch update , 得到

需要修改jdbc连接，增加  allowMultiQueries=true 参数， 然后问题解决



### 解决方案

 正常使用 LocalDateTime , jdbc连接增加 allowMultiQueries=true 

