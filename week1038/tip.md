## Tip - Aiflow注解生成Task的易错点

### Situation

在使用Airflow配置任务时候，使用了Task group 和 task注解来实现任务配置，使用一组注解task用来检测数据是否完整，完整的话就继续走以后逻辑。

### Task

在这样的情况下，本应该检查完数据才进行下一步，结果没有实际检查，因此要查具体原因。

### Action

**查看日志**，实际执行检查会打出相应日志```Success criteria met. Exiting.```， 但我的任务日志并没有检查，

任务代码架构如下:

```python
with DAG () as dag:
		with TaskGroup(...) as group:
			@task
			def check_xxx():
				S3KeySensor(...)
		...
    start >> group >> logical_task >> end
    
		
```



**找更有经验的同事询问**。  为何这样会导致S3Keysor没有执行？告知，可能是bug吧，没遇见过。

**找实现过类似功能的代码**。 找了别人写的非注解方式实现的代码， 改造此功能后如下:

```python
with DAG () as dag:

			check_a = PythonOperator(...)
    	check_b = PythonOperator(...)
		...
    start >> [check_a, check_b] >> logical_task >> end
    
		
```

发现是可以正常执行的，能打出相应日志。

**对比实现**。 对比两个实现版本， 我写的不同点是引入TaskGroup, 和注解 @task， TaskGroup不太可能出问题，因为我有其他这样写的任务没有报错，问题大概率出现在@task上。

**验证问题** @task 其实是对PythonOperator简化写法， 去掉@task改成原生模式:

```python

def check_a_callable():
		S3KeySensor(...)
def check_b_callable():
		S3KeySensor(...)
		
with DAG () as dag:
		def
		with TaskGroup(...) as group:
			 p1 = PythonOperator(... callable=check_axxx)
			 p2 = ...
		...
    start >> group >> logical_task >> end
```

这里其实已经能看出问题了， check_a_callable()中，实际执行的是方法体的 S3KeySenor构造方法，并没有执行S3KeySensor内部的方法。 

### Result 结果

去官网找S3KeySensor的文档和用法， 发现官网写的很明白， S3KeySensor就是一种Operator, 并不需要对外层再包一个Operator！

所以，最后的写法就是去掉@task ：

```python
with DAG () as dag:
		with TaskGroup(...) as group:
				p1 = S3KeySensor(...)
        p2 = S3KeySensor(...)
		...
    start >> group >> logical_task >> end
    
```



