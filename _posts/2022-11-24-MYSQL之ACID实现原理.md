---
layout: post
title: MYSQL之ACID实现原理
category: 数据库原理
tags: [mysql]
description: 数据库原理
keywords: 数据库原理
---

介绍mysql工作原理，ACID的实现


## 基本概念

- 原子性（Atomicity）

原子性确保事务是一个不可分割的工作单位，要么全部执行成功，要么全部失败回滚。如果事务在执行过程中发生错误，系统会将其恢复到事务开始前的状态，以保持数据库的一致性。


- 一致性（Consistency）

一致性确保事务使数据库从一个一致状态变为另一个一致状态。事务在执行过程中，必须遵守预定义的规则和约束，以保证数据的完整性和正确性。


- 隔离性（Isolation）

隔离性确保事务之间是相互隔离的，使得在一个事务看到的数据对其他事务是不可见的。这防止了事务之间的干扰和数据竞争。


- 持久性（Durability）

持久性确保一旦事务提交，其结果将永久保存在数据库中，即使发生系统崩溃或故障，数据也不会丢失。


ACID 4 个特性中：一致性（consistency）是目的；原子性（atomicity）、隔离性（isolation）、持久性（durability）是手段。


## 工作原理

### 原子性

事务通常由多个语句组成，原子性保证每个事务都被视为一个单独的单元,要么完全成功，要么完全失败。即一个事务（transaction）中的所有操作，要么全部执行成功，要么全部不执行。

简单来说：原子性的结果就是没有中间状态，如果有中间状态则一致性就不会得到满足。

仔细想一想没有中间结果是不可能实现的，所以 MySQL 采用了曲线救国的方式：即执行失败后，可以回滚，保证不会出现部分成功的情况，通过 undolog 实现该特性。

事务在执行过程中发生错误，会被回滚（Rollback）到事务开始前的状态，就像这个事务从来没有执行过一样。


实现原理


1.通过 undolog 在失败时回滚保证在结果上是原子性的， 即没有中间状态。
undo log 属于逻辑日志，它记录的是 sql 执行相关的信息。当发生回滚时，InnoDB 会根据 undo log 的内容做与之前相反的工作：对于每个 insert，
回滚时会执行 delete；对于每个 delete，回滚时会执行insert；对于每个 update，回滚时会执行一个相反的 update，把数据改回去。

2.通过隔离性保证了在其他并发事务看来是原子性的，即中间状态对外不可见。


### 一致性


一致性是指事务执行结束后，数据库的完整性约束没有被破坏，事务执行的前后都是合法的数据状态。

数据库的完整性约束包括但不限于：实体完整性（如行的主键存在且唯一）、列完整性（如字段的类型、大小、长度要符合要求）、外键约束、用户自定义完整性（如转账前后，两个账户余额的和应该不变）。

可以说，一致性是事务追求的最终目标：前面提到的原子性、持久性和隔离性，都是为了保证数据库状态的一致性。此外，除了数据库层面的保障，一致性的实现也需要应用层面进行保障。

### 隔离性

隔离性，指一个事务内部的操作及使用的数据对正在进行的其他事务是隔离的，并发执行的各个事务之间不能互相干扰。隔离性可以防止多个事务并发执行时由于交叉执行而导致数据的不一致。

正是它保证了原子操作的过程中，中间状态对其它事务不可见。

Mysql 隔离级别有以下四种（级别由低到高）：

- ReadUncommitted 读未提交

最低的隔离级别，一个事务可以读取其他事务未提交的数据。可能出现的问题包括脏读、不可重复读和幻读。

- ReadCommitted 读已提交

Oracle默认隔离级别。一个事务只能读取已提交的数据。可能出现的问题包括不可重复读和幻读。

- RepeatableRead 可重复读

确保在同一个事务中多次读取同样的数据时，会得到一致的结果。可能出现的问题是幻读。

- Serializable 串行化

最高的隔离级别，完全隔离事务，确保不会出现脏读、不可重复读和幻读的问题。但是并发性能较差。


可能出现的问题：

脏读（Dirty Read）：一个事务读取了另一个事务未提交的数据。这可能导致事务基于不准确的数据做出决策。

不可重复读（Non-repeatable Read）：在同一个事务内，读取相同数据的两次读取结果不一致。这可能导致数据的一致性问题。

幻读（Phantom Read）：在同一个事务内，两次查询返回的数据行数不一致。这可能导致事务基于不完整的数据做出决策。


隔离性追求的是并发情形下事务之间互不干扰。简单起见，我们主要考虑最简单的读操作和写操作(加锁读等特殊读操作会特殊说明)，那么隔离性的探讨，主要可以分为两个方面：

- (一个事务)写操作对(另一个事务)写操作的影响：锁机制保证隔离性

- (一个事务)写操作对(另一个事务)读操作的影响：MVCC保证隔离性

### 持久性


持久性是指事务一旦提交，它对数据库的改变就应该是永久性的。接下来的其他操作或故障不应该对其有任何影响。

InnoDB 通过 redo log 重做日志保证了事务的持久性。

事务开始之后就产生 redo log，redo log 的落盘并不是随着事务的提交才写入的，而是在事务的执行过程中，便开始写入 redo log 文件中。

参数innodb_flush_log_at_trx_commit可设置在事务 commit 的时候必须要写入 redo log 文件。

redo log 具体原理：

- 当数据修改时，除了修改 Buffer Pool 中的数据，还会在 redo log 记录这次操作；

- 当事务提交时，会调用 fsync 接口对 redo log 进行刷盘。

- redo log 采用的是 WAL（Write-ahead logging，预写式日志），所有修改先写入日志，再更新到 Buffer Pool 。

- 如果 MySQL 宕机，重启时可以读取 redo log 中的数据，对数据库进行恢复，保证了数据不会因 MySQL 宕机而丢失，从而满足了持久性要求。




