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
### 三种问题
#### 脏读
读到事务操作的中间结果
#### 不可重复读
事务中第一次读取到的数据集和第二次读取到的数据集不同
#### 幻读
解决可重复读问题时，第一次读取数据时，会将引用到的行数据锁定，直到整个事务结束。但是无法锁定未指定的行。
举例：
1. A 事务，第一次 查询 2019 年之前的记录，得到一个结果集，然后对这个结果集全部做了修改。
2. B 事务，向表内添加了一条 2018 年的记录，修改了结果集
3. A 事务不知道 B 事务的行为，A 仍认为 2019年之前的事务都已经全部修改成功，然后重新查找 2019 年之前的数据，会发现一个没有修改过的 B 插入的记录，因此发生了幻读。


### 四种隔离级别
#### 1. 读未提交
- 最基本问题：会发生脏读
- 解决办法：将事务未完成期间设置成不可见，保证事务的原子性。

##### mysql 测试读未提交会发生脏读
首先创建数据库 test，在 test 中创建 student 表，student 表中有三个字段，分别是 ID, NAME, AGE。
```sql
+-------+-------------+------+-----+---------+----------------+
| Field | Type        | Null | Key | Default | Extra          |
+-------+-------------+------+-----+---------+----------------+
| ID    | int(11)     | NO   | PRI | NULL    | auto_increment |
| NAME  | varchar(20) | NO   |     | NULL    |                |
| AGE   | int(11)     | NO   |     | NULL    |                |
+-------+-------------+------+-----+---------+----------------+
```
打开两个终端，将终端 session 的隔离级别修改成读未提交，A 终端负责将 ID = 3 的学生的姓名修改成 'wang'，休眠 10 s 后，再将其修改成 'sun'。B 终端在 A 终端开启之后查询 ID = 3 的学生的姓名。可以发现 ID = 3 的学生的姓名最终是 'sun'，但是 B 终端查询到的却是 'wang'。

A 终端 sql 语句
```sql
USE test;
SET autocommit = 0; -- 禁止自动提交
SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED; -- 将 session 的隔离级别设置成读未提交
-- SELECT @@tx_isolation;
START TRANSACTION;
UPDATE student SET  NAME = 'wang' WHERE ID = '3';
select sleep(10); -- 休眠 10 s，给 B 客户端充足的时间去查询
UPDATE student SET  NAME = 'sun' WHERE ID = '3';
COMMIT;
```

B 终端 SQL 语句
```sql
USE test;
SET autocommit = 0; -- 禁止自动提交
SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED; -- 将当前 session 设置成读未提交
-- SELECT @@tx_isolation;
START TRANSACTION;
SELECT NAME FROM student WHERE id = 3;
COMMIT;
```
B 终端的查询结果为
```sql
+------+
| name |
+------+
| wang |
+------+
```
B 事务读取到了 A 事务的中间结果。

#### 2. 读已提交
事务原子化，B 事务完全提交后，A 事务才可以读取。将读未提交中的测试用例中的数据隔离级别提升到读已提交（READ COMMITTED），就不会读取到中间结果了，虽然能够解决脏读问题，但是无法解决重复读问题。

##### mysql 测试读已提交无法解决重复读问题
操作流程是这样的，A 事务连续读两次，中间睡眠 10 s，B 事务修改内容。A 事务先启动，B 事务后启动。

A 事务

```sql
-- A 事务修改
USE test;

-- 创建查询结果，用来保存 A 事务的两次查询结果
CREATE TABLE result IF NOT EXISTS(
result VARCHAR(12)
);

-- 清空结果表
DELETE FROM result;
SET autocommit = 0; -- 禁止自动提交
SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;
-- SELECT @@tx_isolation;
START TRANSACTION;
INSERT INTO result (SELECT NAME FROM student WHERE id = '3');
SELECT SLEEP(10);
INSERT INTO result (SELECT NAME FROM student WHERE id = '3');
COMMIT;
```

B 事务
```java
-- B 事务修改
USE test;

SET autocommit = 0; -- 禁止自动提交
SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED; -- 将当前 session 设置成读未提交

-- SELECT @@tx_isolation;

START TRANSACTION;
UPDATE student SET  NAME = 'zhang' WHERE ID = '3';
COMMIT;
```
查询 result 的结果，发现两次的结果不同的，证明不能重复读。
```sql
+--------+
| result |
+--------+
| sun    |
| zhang  |
+--------+
```


#### 3. 可重复读
事务 A 中，对数据集的两次读取操作得到的数据集相同，只针对 A 操作过的数据。使用 MVCC 机制就能保证可重复读，重复上面的实验，B 事务会一直在等 A 事务完成，最终 A 事务得到的数据都是相同的。但是无法解决幻读问题。

##### MVCC(多版本控制并发)
为了实现提交读和可重复读，InnoDB 使用 MVCC 机制。
###### 版本号
- 系统版本号，假定为 n_s
- 事务版本号，假定为 n_t

每一个数据行都有两个隐藏列，一个存放创建版本号，一个存放删除版本号
- 创建版本号，假定为 n_c
- 删除版本号，假定为 n_d

###### 系统版本号和事务版本号的获取规则
1. 系统版本号，是一个递增的数字，每次开启一个事务，递增一个版本号。
2. 事务版本号，事务开始时系统版本号。

###### **实现过程**
1. SELECT
当前事务版本号为 n_t，如果
    - n_t > n_c，有效，说明的当前事务读取到的行是当前事务开始之前创建的数据行
    - n_t <= n_c，无效，说明当前事务开始后，有别的事务对数据行进行了修改，事务回滚
    - n_t < n_d，有效，说明是事务开始之后，才被其他事务删除
    - n_t <= n_d，无效，说明事务开始之前，该数据行已经被删除，不应该去读取这个

2. INSERT
当前行的 n_c = n_s

3. DELETE
当前行的 n_d = n_s

4. UPDATE
当前行的 n_d = n_s，n_c = n_s

###### 回滚
使用 Undo 日志将每个事务的快照保存下来，通过回滚指针将所有快照连接起来，以便回滚。

##### 测试 mvcc 机制
尝试进行测试时发现事务 B 会被阻塞，看起来像是加锁了，也就是说 Mysql innodb 版本实际上并没有仅仅使用 mvcc 机制来保证可重复读。此处存疑。

#### 4. 串行化读
事务执行串行化。直接加锁，但是加锁有可能造成死锁现象。


[](下一步，高性能的mysql，深入浅出 mysql，mysql 手册)