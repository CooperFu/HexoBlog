title: Mysql, 一条语句是怎么执行的?
date: 2018-11-18 16:10:56
tags: [MySQL, 关系型数据库]
---

`select * from table where id=2;`

上面这条语句是怎么执行的呢?

![](https://static001.geekbang.org/resource/image/0d/d9/0d2070e8f84c4801adbfa03bda1f98d9.png)


### 连接器

首先mysql会连接mysql服务器, 通常是用 `mysql -h$ip -P$port -u$user -p` 的方式连接

最后一个-p是密码, 不推荐明文直接输入, 上面的方式按回车后, 系统会让你输入密码, 然后根据内置的权限表查看当前用户是否有相关权限.

默认保持一个长链接, 有一个参数是wait_timeout 默认是8小时.

- 会有长连接相关问题.

### 查询缓存

- 通过输入的查询语句, 先会去查询缓存, 看看是否之前有查询过一模一样的语句, 有hit就直接返回.
- 缓存策略大概是key-value的形式, key是查询语句, value是返回结果. key-value的好处是复杂度是O(1). 
- 但是基本很难命中, 因为但凡一个表中任意一条数据的一个小字段更新了, 那么这张表的缓存基本作废. 就会被移出掉.
- 缓存例外基本是静态表, 比如一张配置表, 很久都不会有人去更新他, 那么缓存的作用就大大增加.
- 可以直接指定查询缓存, `select SQL_CACHE * from table where id=2;` 但是在mysql 8.0之后 这个特性就去掉了.

### 分析器
分析器顾名思义, 就是分析输入的语句, 变成格式化的dsl

`select * from table where id=2;`

就会分析 是要从 table这张表中 找  列id的值等于2 的结果.

如果打错, 比如select打成了 selct, 就会报错, syntax error.

### 优化器

优化器是在表里有多个索引的时候, 决定使用哪个索引; 或者一条语句中有多表关联的时候, 决定连接顺序. 比如:

`select * from t1 join t2 using(ID) where t1.c=10 and t2.d=20;`

- 既可以先从表 t1 里面取出 c=10 的记录的 ID 值，再根据 ID 值关联到表 t2，再判断 t2 里面 d 的值是否等于 20。
- 也可以先从表 t2 里面取出 d=20 的记录的 ID 值，再根据 ID 值关联到 t1，再判断 t1 里面 c 的值是否等于 10。

上面两种方法的结果是一样的, 但是效率会有所不同. 优化器就是按需决定使用哪种方案.


### 执行器

执行器就是将决定好的方案进行执行
- 先会根据表名来判断权限, 如果当前用户没有读取这张表的权限 就会直接返回 `permission denied for table 'xxxx'`
- 有权限就会根据mysql的执行过程, 开始逐行扫描. 只到返回结果.


### 小问题

> 如果表 T 中没有字段 k，而你执行了这个语句 select * from T where k=1, 那肯定是会报“不存在这个列”的错误： “Unknown column ‘k’ in ‘where clause’”。你觉得这个错误是在我们上面提到的哪个阶段报出来的呢？


答案是分析器, 有的人可能以为是执行器, 因为只有执行才知道有没有这个列, 但是错, 不需要读取表就可以知道, mysql有一张schema的表就是专门做这个事儿的. 
