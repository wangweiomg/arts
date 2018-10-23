## ARTS - Tip
## MyBatis 的 if test="param == '1'" 的问题

在项目开发中，发现了mybatis的一个问题，如下：

```
		<if test="first == 1">
            limit 1
        </if>
        <if test="second == '2'">
            limit 2
        </if>
```

如果第一个没问题，第二个 =='2' 就不会执行，不管参数是integer,int,还是String, 第二个都不会执行，程序也不会报错，查了资料，mybatis 使用了ognl表达式，'2'会被解析成char， 在Java中char 和String 相比较就会不等。

### 改进方法：
#### 1. 增加toString

```
	<if test="second == '2'.toString()">
            limit 2
        </if>
```


#### 2. 双引号
```
	<if test='second == "2"'>
            limit 2
        </if>
```

