---
title: JAVA基础-事务
tags: [JAVA基础,事务]
toc: true
---
# 事务
## 含义
事务是一步或多步组成操作序列组成的逻辑执行单元，这个序列要么全部执行，要么则全部放弃执行。
事务的四个特性
- 原子性 Atomicity
- 一致性 Consistency
- 隔离性 IsoIation
- 持续性 Durability

原子性（Atomicity）：事务应用最小的执行单元，不可再分。是事务中不可再分的最小逻辑执行体。   
一致性（Consistency）：事务的执行结果，必须使数据库的从一个一致性的状态变到另一个一致性的状态。   
隔离线（IsoIation）：各个事务的执行互不干扰，任意一个事务的内部操作对其他并发的事务，都是隔离的。也就是：并发执行的事务之间不能看到对方的中间状态，并发执行的事务之间不能互相影响。   
持续性（Durability）：持续性也称为持久性（Persistence），指事务一旦提交，对数据所做的任何改变，都要记录到永久存储器中，通常就是保存在物理数据库中。

## 事务并发的问题
- 脏读 dirty read
- 不可重复读 non-repeatable read
- 幻读 phantom read

脏读（dirty read）：一个事务读取了另一个事务尚未提交的数据，而这个数据是有可能回滚。   

不可重复读（non-repeatable read）：一个事务的操作导致另一个事务前后两次读到不同的数据。不可重复读意味着，
在数据库访问中，一个事务范围内两个相同的查询却返回了不同数据。这是由于查询时系统中其他事务修改的提交而引起的。   

幻读（phantom read）：一个事务的操作导致另一个事务前后两次查询的结果数据量不同。幻读,是指当事务不是独立执行时发生
的一种现象，例如第一个事务对一个表中的数据进行了修改，这种修改涉及到表中的全部数据行。
同时，第二个事务也修改这个表中的数据，这种修改是向表中插入一行新数据。那么，
以后就会发生操作第一个事务的用户发现表中还有没有修改的数据行，就好象发生了幻觉一样.。   


## 事务的隔离级别
在数据库系统中，事务的隔离级别（Isolation ）决定了该事务的完整性，低级别的事务隔离级别增强了多用户同时访问数据的能力，
但是也加大了并发产生负面影响的可能性（脏读，丢失更新等等）。
标准的事务隔离级别定义如下：
### Serializable 
最高的隔离级别，在基于锁机制的并发控制的数据库系统中，Serializable要求在选中数据上的读锁和写锁在事务结束时才释放。
在不基于锁的并发控制机制中，系统检测到事务有写冲突时，只有一个事务能够提交。
### Repeatable reads
基于锁的并发控制中，会对选定的对象进行读锁和写锁，但没有范围锁，因此会有幻读。
### Read committed
基于锁的并发控制中，选中对象的写锁会一直保持到事务结束，但是读锁在SELECT语句后会立即释放，会产出不可重复读。

### Read uncommitted
最低级别的隔离级别，允许脏读，其他事务可以看到当前事务未提交的修改。

# Java JDBC事务机制
JDBC的事务基于Connection，通过Connection对象对事务进行管理。`setAutoCommit`、`commit`、`rollback`等。
- 自动提交模式 auto-commit mode，auto-commit 为true
- auto-commit为false，每个事务都必须显示调用commit方法进行提交，或者显示调用rollback方法进行回滚


## 事务隔离级别（Transaction Isolation Levels）
- TRANSACTION_NONE JDBC驱动不支持事务
- TRANSACTION_READ_UNCOMMITTED 允许脏读、不可重复读和幻读
- TRANSACTION_READ_COMMITTED 禁止脏读，但允许不可重复读和幻读
- TRANSACTION_REPEATABLE_READ 禁止脏读和不可重复读，允许幻读
- TRANSACTION_SERIALIZABLE 禁止脏读、不可重复读和幻读

