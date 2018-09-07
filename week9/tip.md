## ARTS - Tip

### 关于MyBatis 报错的一个反思

今天开发一个功能，和别的系统功能重复，就直接复制了别的系统的Mapper，写好业务逻辑后测试，发现系统报异常：

```

Caused by: org.apache.ibatis.reflection.ReflectionException: There is no getter for property named 'id' in 'class java.lang.String'


```

意思是，String类里没有 名字为 id 的属性的get 方法。我有点奇怪，我调用Mapper的是一个 insert 方法，传的是业务对象，为什么会报String的id问题？有点奇怪，于是就进行了谷歌、百度大法，几乎所有的帖子都是如下：


> 传入的参数是String， parameterType="String"，解决方法是：


> 解决方法一:把#{xxx}修改为 #{_parameter} 即可

>解决方法二:可以在方法中提前定义:@Param(value="xxx") String xxx
>


虽然报错和我的一样，但是明显不是我这样的问题情形。于是又检查了一遍代码，发现了问题所在：

```
INSERT INTO sys_log(
			id,
			type,
			title,
			create_by, 
			create_date
			
		) VALUES (
			#{id},
			#{type},
			#{title}, 
			#{createBy.id},
			#{createDate}
		)


```


居然是 **#{createBy.id}** 。 检查了下原来代码并没有CreateBy 对象且有属性是id, 所以觉得是原系统本来就有误。 处理就很简单了，删了id即可，代码正常运行。


### 教训
1. 参考别的系统可以，最好不要直接复制。
2. 就算复制也需要批判的使用，毕竟不是久经考验的公共库
3. 不要偷懒，多自己实现
