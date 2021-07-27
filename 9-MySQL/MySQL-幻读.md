## 幻读-从幻读问题讲mysql`Innodb`锁
> 本次讨论`幻读`问题的基础:
> - 数据库 MySQL
> - 引擎 Innodb
> - 事务的隔离级别 可重复读 `REPEATABLE-READ`

##### 1.引入
> RR(可重复读)可以解决幻读问题吗?

##### 2.定义, 什么是幻读?
官方文档 `mysql 5.6 reference manual`: <br>
> The so-called phantom problem occurs within a transaction when the same query produces different sets of rows at different times. For example, if a SELECT is executed twice, but returns a row the second time that was not returned the first time, the row is a “phantom” row.<br>


由此可见，幻读是一个事务内两次相同的查询语句在不同时间读取到的数据不一致问题.<br>


##### 3.问题引入, 快照读还是当前读?

但是，文档中的定义有一个问题,该查询是**快照读**还是**当前读**?

答案： **当前读**

```mysql
    select * from t where id=10 for update;
```

众所周知,`Innodb`引擎支持`MVCC`(多版本并发控制),普通SQL的SELECT都是快照读,每次读取都只是读取当前版本的数据,这也是可重复读的原因.如果幻读中定义的SQL读取为快照读,那么也不存在幻读问题了.所以,幻读的问题针对的是**当前读**.<br>

在一个事务中,如果想要读取最新的数据,就需要加X锁(排它锁),即 select .... for update.
普通的行锁显然不足以解决INSERT导致的幻读问题,这里引入netx-key lock.





