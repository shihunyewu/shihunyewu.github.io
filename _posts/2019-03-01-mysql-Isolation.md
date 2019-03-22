---
layout:     post
title:      "Mysql-Isolation"
subtitle:   " \"Mysql 中的隔离级别\""
date:       2019-03-01 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - MySql
---
> Mysql 中的隔离级别

## Mysql 中的隔离级别
### 四种隔离级别
##### 读未提交
A 事务读取数据集时， B 事务对数据集修改后，还未提交，但是 A 事务已经能够读取到 B 事务级对数据集的修改。这里存在的问题也叫做脏读。
##### 读已提交
事务原子化，B 事务完全提交后，A 事务才可以读取。能够解决脏读问题，无法解决重复读问题。
##### 可重复读
事务 A 中，对数据集的两次读取操作得到的数据集相同。只针对 A 操作过的数据。无法解决幻读问题。
##### 串行化读
事务执行串行化。

### 三种问题
##### 脏读
读到事务操作的中间结果
##### 不可重复读
事务中第一次读取到的数据集和第二次读取到的数据集不同
##### 幻读
解决可重复读问题时，第一次读取数据时，会将引用到的行数据锁定，直到整个事务结束。但是无法锁定未指定的行。
举例：
1. A 事务，第一次 查询 2019 年之前的记录，得到一个结果集，然后对这个结果集全部做了修改。
2. B 事务，向表内添加了一条 2018 年的记录，修改了结果集
3. A 事务不知道 B 事务的行为，A 仍认为 2019年之前的事务都已经全部修改成功，然后重新查找 2019 年之前的数据，会发现一个没有修改过的 B 插入的记录，因此发生了幻读。