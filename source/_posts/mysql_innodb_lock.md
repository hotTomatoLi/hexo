# InnoDB Locking
这里描述了Innodb中使用的锁。
- Shared and Exclusive Locks 共享锁和排他锁
- Intention Locks 意向锁
- Record Locks 行锁
- Gap Locks 间隙锁
- Next-Key Locks next-key锁
- Insert Intention Locks 插入意向锁
- AUTO-INC Locks 自增锁

## Shared and Exclusive Locks
InnoDB实现了标准的行级别锁，现在有两种类别的锁，共享锁（shared locks,S locks），排他锁（exclusive locks, X locks）。
- 共享锁   
允许持有锁的事务读取一行数据；
共享锁就是多个事务对于同一数据可以共享一把锁，都能访问到数据，但是只能读不能修改；
- 排他锁
排他锁允许持有锁的事务更新或是删除一行数据

如果事务T1持有一行数据r的S锁，其他事务T2发起的针对该行的事务如下：
- 如果T2请求的是一个S锁，那么T2会马上获得S锁，T1和T2都会持有r的S锁；（s锁个数？）
- 如果T2请求的是一个X锁，那么T2不会马上获得锁；

如果T1持有r的X锁，T2如果请求获得任意锁，都不会马上得到，T2会等待T1释放锁。

## Intension Locks意向锁
>InnoDB supports multiple granularity locking which permits coexistence of row-level locks and locks on entire tables. 
To make locking at multiple granularity levels practical, additional types of locks called intention locks are used.

InnoDB 支持了多粒度锁机制，允许行锁和表锁共存。为了实现上述多粒度锁机制，InnoDB使用了意向锁。

>Intention locks are table-level locks in InnoDB that indicate which type of lock (shared or exclusive) a transaction 
will require later for a row in that table. 
There are two types of intention locks used in InnoDB (assume 
that transaction T has requested a lock of the indicated type on table t):
Intention shared (IS): Transaction T intends to set S locks on individual rows in table t.
Intention exclusive (IX): Transaction T intends to set X locks on those rows.

在InnoDB中，意向锁是表级别的锁，也就是说，在一张表中，   
在InnoDB中，有两种意向锁，（假设事务T已经请求了表t的某种锁）：
- 意向共享锁，IS锁：事务T想要对表t中个别的行加上s锁；
- 意向排他锁，IX锁：事务T想要对那些行集上X锁。

例如`SELECT ... LOCK IN SHARE MODE` 设置了IS锁， `SELECT ... FOR UPDATE` 设置了IX锁。

意向锁协议如下：
- 一个事务，如果可以获得表t的一行的S锁(即加上S锁)，那么该事务必须首先获得表t的IS锁或是更强的锁；
- 一个事务，如果可以获得一行数据的X锁(即加上X锁)，那么它必须首先获得表t的IX锁。

锁兼容矩阵

|请求锁模式<br>是否兼容<br>当前锁模式 |X |IX | S | IS |
|--|--|--|--|--|
|X|冲突 |冲突 | 冲突 | 冲突 |
|IX |冲突|兼容|冲突| 兼容|
|S|冲突|冲突|兼容|兼容|
|IS|冲突|兼容|兼容|兼容| 

如果请求锁的事务的请求模式与当前锁模式兼容，请求的事务可以获得锁。否则不会获得锁，将会等待冲突的当前锁释放。如果请求锁与现有锁模式冲突，
不会获得锁，因为那样会造成死锁，甚至错误。


http://dev.mysql.com/doc/refman/5.7/en/innodb-locking.html
http://www.myexception.cn/mysql/1712377.html
http://blog.csdn.net/xifeijian/article/details/20313977