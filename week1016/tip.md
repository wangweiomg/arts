## ARTS - Tip
## 一个SQL JOIN问题

### 问题描述

工作中碰到了一个SQL JOIN 的bug。

典型的内部系统， 牵涉到的用户表，用户角色表，用户角色关联表，分别是： ```sys_user, sys_role, sys_user_role```

需求是获得 角色类型为 xxx 的所有用户的id, 名字 和角色id.

我写的SQL是如下


```
SELECT
      u.id,
      u.name ,
      ur.role_id 
FROM sys_user u
  LEFT JOIN sys_user_role ur on u.id = ur.user_id
                  AND ur.role_id IN 
                  (SELECT id FROM sys_role r WHERE r.role_type = 'xxx')
```

### 问题分析

根据上面SQL，做的事情是

查找所有的用户表，  关联上 用户角色表， 这个用户角色表数据只有角色ID 是 role_type = xxx 的数据。连接用的是LEFT JOIN，这样的话，左边的数据是 >= 右边的数据，左边会出现一些无用数据。

出现使用LEFT JOIN 是对连接的范围理解的有误。 关于各种连接，下面一张图就能看明白：

![](https://i.stack.imgur.com/1UKp7.png)

### 问题解决

把上面的SQL 改成 INNER JOIN 即可解决。这样就是等于把左边的范围缩小到和右边数据对等的数量上。

当然也有直接面对问题的写法， 按照逻辑， 先找出 ```role_type=xxx```的角色id, 然后去关联表```sys_user_role```查找所有的 ```role_id ```等于刚才查出来的角色ID 的数据，取出用户ID，最后在用户表根据ID查出所有用户，SQL如下：

```
select
  u.id,
  u.name
from sys_user u
where u.id In (
  select user_id
  from sys_user_role
  where role_id In (
    select id
    from sys_role
    where role_type = 'xxx'));
```


