# 第七讲 MySQL的事务
## MySQL事务的概念
mysql中，事务其实是一个最小的不可分割的工作单元。事务能保证一个业务的完整性。
```
比如我们银行转账：
a -> -100
update user set money=money-100 where name='a';

b -> +100
update user set money=money+100 where name='b';

-- 实际程序中，如果只有一条语句执行成功，而另外一条没有执行成功，出现数据前后不一致。

update user set money=money-100 where name='a';
update user set money=money+100 where name='b';

-- 多条sql语句，可能会有同时成功的要求，要么就同时失败。
```
## MySQL控制事务
1.mysql是默认开启事务的(自动提交)。
```
mysql> select @@autocommit;
+--------------+
| @@autocommit |
+--------------+
|            1 |
+--------------+
1 row in set (0.00 sec)
```
2.默认事务开启的作用：当我们去执行一个sql语句的时候，效果会立即体现出来，且不能回滚。
## MySQL事务的相关实践
### 默认开启事务(开启自动提交)
1.新建数据库
```
create database bank;
```
2.创建user表
```
mysql> create table user(id int primary key,name varchar(20),money int);
Query OK, 0 rows affected (0.02 sec)
```
3.更改字符集
```
mysql> alter table user change name name varchar(20) character set utf8;
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0
```
4.插入数据
```
mysql> insert into user values(1,'a',1000);
Query OK, 1 row affected (0.00 sec)
```
5.事务回滚：撤销sql语句执行效果(此时回滚无法撤销插入的数据)
```
mysql> rollback;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from user;
+----+------+-------+
| id | name | money |
+----+------+-------+
|  1 | a    |  1000 |
+----+------+-------+
1 row in set (0.00 sec)
```
### 关闭自动提交
1.设置mysql自动提交为false(关闭了mysql的自动提交)
```
mysql> set autocommit=0;
Query OK, 0 rows affected (0.00 sec)

mysql> select @@autocommit;
+--------------+
| @@autocommit |
+--------------+
|            0 |
+--------------+
1 row in set (0.00 sec)
```
2.插入数据
```
mysql> insert into user values(2,'b',1000);
Query OK, 1 row affected (0.00 sec)

mysql> select * from user;
+----+------+-------+
| id | name | money |
+----+------+-------+
|  1 | a    |  1000 |
|  2 | b    |  1000 |
+----+------+-------+
2 rows in set (0.00 sec)
```
3.事务回滚(此时插入数据操作被撤销)
```
mysql> rollback;
Query OK, 0 rows affected (0.00 sec)

-- 结果
mysql> select * from user;
+----+------+-------+
| id | name | money |
+----+------+-------+
|  1 | a    |  1000 |
+----+------+-------+
1 row in set (0.00 sec)
```
### 手动提交
1.插入数据
```
mysql> insert into user values(2,'b',1000);
Query OK, 1 row affected (0.00 sec)
```

2.手动提交数据
```
mysql> commit;
Query OK, 0 rows affected (0.01 sec)
```

3.再撤销，是不可以撤销的(持久性)
```
mysql> rollback;
Query OK, 0 rows affected (0.00 sec)

-- 结果
mysql> select * from user;
+----+------+-------+
| id | name | money |
+----+------+-------+
|  1 | a    |  1000 |
|  2 | b    |  1000 |
+----+------+-------+
2 rows in set (0.00 sec)
```

### MySQL事务的相关操作
#### 关闭自动提交,回滚有效(事务给我们提供一个返回的机会)
1.转账操作
```
mysql> update user set money=money-100 where name='a';
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> update user set money=money+100 where name='b';
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0

-- 结果
mysql> select * from user;
+----+------+-------+
| id | name | money |
+----+------+-------+
|  1 | a    |   900 |
|  2 | b    |  1100 |
+----+------+-------+
2 rows in set (0.00 sec)
```

2.事务回滚
```
mysql> rollback;
Query OK, 0 rows affected (0.00 sec)

-- 回滚之后的结果
mysql> select * from user;
+----+------+-------+
| id | name | money |
+----+------+-------+
|  1 | a    |  1000 |
|  2 | b    |  1000 |
+----+------+-------+
2 rows in set (0.00 sec)
```
#### 开启自动提交，回滚无效
1.先把默认事务开启打开
```
mysql> set autocommit=1;
Query OK, 0 rows affected (0.00 sec)

mysql> select @@autocommit;
+--------------+
| @@autocommit |
+--------------+
|            1 |
+--------------+
1 row in set (0.00 sec)
```
2.查看数据
```
mysql> select * from user;
+----+------+-------+
| id | name | money |
+----+------+-------+
|  1 | a    |  1000 |
|  2 | b    |  1000 |
+----+------+-------+
2 rows in set (0.00 sec)
```
3.转账操作
```
mysql> update user set money=money-100 where name='a';
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> update user set money=money+100 where name='b';
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select * from user;
+----+------+-------+
| id | name | money |
+----+------+-------+
|  1 | a    |   900 |
|  2 | b    |  1100 |
+----+------+-------+
2 rows in set (0.00 sec)
```
4.事务回滚
```
mysql> rollback;
Query OK, 0 rows affected (0.00 sec)

-- 没有被撤销
mysql> select * from user;
+----+------+-------+
| id | name | money |
+----+------+-------+
|  1 | a    |   900 |
|  2 | b    |  1100 |
+----+------+-------+
2 rows in set (0.00 sec)
```
#### 手动开启事务(此时自动提交已经关闭)
 begin;或者start transaction;(都可以手动开启一个事务)
- 第一种情况
```
1.手动开启事务
mysql> begin;
Query OK, 0 rows affected (0.00 sec)

2.转账操作
mysql> update user set money=money-100 where name='a';
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> update user set money=money+100 where name='b';
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0


mysql> select * from user;
+----+------+-------+
| id | name | money |
+----+------+-------+
|  1 | a    |   800 |
|  2 | b    |  1200 |
+----+------+-------+
2 rows in set (0.00 sec)

3.事务回滚(此时转账操作被撤销成功)
mysql> rollback;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from user;
+----+------+-------+
| id | name | money |
+----+------+-------+
|  1 | a    |   900 |
|  2 | b    |  1100 |
+----+------+-------+
2 rows in set (0.00 sec)
```
- 第二种情况
```
1.手动开启事务
mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)

2.转账操作
mysql> update user set money=money-100 where name='a';
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> update user set money=money+100 where name='b';
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select * from user;
+----+------+-------+
| id | name | money |
+----+------+-------+
|  1 | a    |   800 |
|  2 | b    |  1200 |
+----+------+-------+
2 rows in set (0.00 sec)

3.事务回滚(此时转账操作被撤销成功)
mysql> rollback;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from user;
+----+------+-------+
| id | name | money |
+----+------+-------+
|  1 | a    |   900 |
|  2 | b    |  1100 |
+----+------+-------+
2 rows in set (0.00 sec)
```
- 第三种情况
事务开启之后，一旦commit提交，就不可以回滚。
```
1.手动开启事务
mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)

2.转账操作
mysql> update user set money=money-100 where name='a';
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> update user set money=money+100 where name='b';
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

3.手动提交
mysql> commit;
Query OK, 0 rows affected (0.01 sec)

mysql> select * from user;
+----+------+-------+
| id | name | money |
+----+------+-------+
|  1 | a    |   800 |
|  2 | b    |  1200 |
+----+------+-------+
2 rows in set (0.00 sec)

4.事务回滚(此时转账操作被撤销失败)
mysql> rollback;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from user;
+----+------+-------+
| id | name | money |
+----+------+-------+
|  1 | a    |   800 |
|  2 | b    |  1200 |
+----+------+-------+
2 rows in set (0.00 sec)
```
#### 手动开启事务操作总结
1.事务手动开启
```
1.修改默认提交 set autocommit;
2.begin;
3.start transaction;
```
2.事务手动提交
```
commit;
```
3.事务手动回滚
```	
rollback;
```
### 事务的四大特性
A 原子性：事务是最小的单位，不可以再分割。  
C 一致性：事务要求，同一事物中的sql语句，必须保证同时成功或者同时失败。  
I 隔离性：事务1和事务2之间是具有隔离性的。  
D 持久性：事务一旦结束(commit)，就不可以返回。  

#### 事务隔离性相关概念
1.read uncommitted;   读未提交的  
2.read committed;     读已经提交的  
3.repeatable read;    可以重复读   
4.serializable;       串行化

#### 事务隔离性实际操作
- 查看数据库的隔离级别
```
# mysql 8.0
-- 系统级别的
select @@global.transaction_isolation;

-- 会话级别的
select @@transaction_isolation;

# mysql 5.x:
-- 会话级别的(mysql默认隔离级别)
mysql> select @@tx_isolation;
+-----------------+
| @@tx_isolation  |
+-----------------+
| REPEATABLE-READ |
+-----------------+
1 row in set, 1 warning (0.00 sec)

-- 系统级别的
mysql> select @global.tx_isolation;
+----------------------+
| @global.tx_isolation |
+----------------------+
| NULL                 |
+----------------------+
```
- 修改隔离级别
```
# mysql 8.0
set global transaction isolation level read uncommitted;

# mysql 5.x
mysql> set session transaction isolation level read uncommitted;
Query OK, 0 rows affected (0.00 sec)

mysql> select @@tx_isolation;
+------------------+
| @@tx_isolation   |
+------------------+
| READ-UNCOMMITTED |
+------------------+
1 row in set, 1 warning (0.00 sec)
```
- read uncommitted
如果有事务a和事务b，a事务对数据进行操作，在操作的过程中，事务没有被提交，但是b可以看见a操作的结果。
```
1.插入数据(bank数据库 user库)

mysql> insert into user values(3,'小明',1000);
Query OK, 1 row affected (0.01 sec)

mysql> insert into user values(4,'淘宝店',1000);
Query OK, 1 row affected (0.00 sec)

mysql> select * from user;
+----+-----------+-------+
| id | name      | money |
+----+-----------+-------+
|  1 | a         |   800 |
|  2 | b         |  1200 |
|  3 | 小明      |  1000 |
|  4 | 淘宝店    |  1000 |
+----+-----------+-------+
4 rows in set (0.00 sec)

2.手动开启事务
mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)

3.转账：小明在淘宝店买鞋子：800块钱。
小明 -> 成都 ATM
淘宝店 -> 广州 ATM

mysql> update user set money=money-800 where name='小明';
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> update user set money=money+800 where name='淘宝店';
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select * from user;
+----+-----------+-------+
| id | name      | money |
+----+-----------+-------+
|  1 | a         |   800 |
|  2 | b         |  1200 |
|  3 | 小明      |   200 |
|  4 | 淘宝店    |  1800 |
+----+-----------+-------+
4 rows in set (0.00 sec)

4.淘宝店确认到账，发货

mysql> select * from user;
+----+-----------+-------+
| id | name      | money |
+----+-----------+-------+
|  1 | a         |   800 |
|  2 | b         |  1200 |
|  3 | 小明      |   200 |
|  4 | 淘宝店    |  1800 |
+----+-----------+-------+
4 rows in set (0.00 sec)

5.小明 -> 成都(执行回滚操作)

mysql> rollback;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from user;
+----+-----------+-------+
| id | name      | money |
+----+-----------+-------+
|  1 | a         |   800 |
|  2 | b         |  1200 |
|  3 | 小明      |  1000 |
|  4 | 淘宝店    |  1000 |
+----+-----------+-------+
4 rows in set (0.01 sec)

6.第二天发现钱少了

mysql> select * from user;
+----+-----------+-------+
| id | name      | money |
+----+-----------+-------+
|  1 | a         |   800 |
|  2 | b         |  1200 |
|  3 | 小明      |  1000 |
|  4 | 淘宝店    |  1000 |
+----+-----------+-------+
4 rows in set (0.00 sec)

7.read uncommitted总结
-- 如果两个不同的地方，都在进行操作，如果事务a开启之后，他的数据可以被其他事务读取到。
-- 这样就会出现(脏读)
-- 脏读：一个事务读到了另一个事务没有提交的数据，就叫做脏读。
-- 实际开发是不允许脏读出现的。
```
- read committed(bank数据库 user表)
```
1.修改隔离级别
-- mysql 8.0
set global transaction isolation level read committed;

-- mysql 5.x
mysql> set session transaction isolation level read committed;
Query OK, 0 rows affected (0.00 sec)

-- 修改隔离级别为read committed
mysql> select @@tx_isolation;
+----------------+
| @@tx_isolation |
+----------------+
| READ-COMMITTED |
+----------------+
1 row in set, 1 warning (0.00 sec)

2.小张：银行会计
-- 手动开启事务
mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from user;
+----+-----------+-------+
| id | name      | money |
+----+-----------+-------+
|  1 | a         |   800 |
|  2 | b         |  1200 |
|  3 | 小明      |  1000 |
|  4 | 淘宝店    |  1000 |
+----+-----------+-------+
4 rows in set (0.00 sec)

3.小王：银行会计
-- 手动开启事务
mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)
-- 插入数据
mysql> insert into user values(5,'c',100);
Query OK, 1 row affected (0.00 sec)
-- 手动提交
mysql> commit;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from user;
+----+-----------+-------+
| id | name      | money |
+----+-----------+-------+
|  1 | a         |   800 |
|  2 | b         |  1200 |
|  3 | 小明      |  1000 |
|  4 | 淘宝店    |  1000 |
|  5 | c         |   100 |
+----+-----------+-------+
5 rows in set (0.00 sec)

4.小张整理平均数

select avg(money) from user;

mysql> select avg(money) from user;
+------------+
| avg(money) |
+------------+
|   820.0000 |
+------------+
1 row in set (0.01 sec)

5.read committed总结
-- money平均值不是1000

-- 虽然我只能读到你提交的数据，但是还是会出现问题，就是
-- 读取同一个表的数据，发现前后不一致。
-- 不可重复读(read committed)
```
- repeatable read(bank数据库 user表)
```
1.修改隔离级别
-- mysql 8.0
set global transaction isolation level repeatable read;

-- mysql 5.x（例子测试在全局隔离级别修改下)
mysql> set session transaction isolation level repeatable read;
Query OK, 0 rows affected (0.00 sec)

mysql> select @@tx_isolation;
+-----------------+
| @@tx_isolation  |
+-----------------+
| REPEATABLE-READ |
+-----------------+
1 row in set, 1 warning (0.00 sec)

mysql> select * from user;
+----+-----------+-------+
| id | name      | money |
+----+-----------+-------+
|  1 | a         |   800 |
|  2 | b         |  1200 |
|  3 | 小明      |  1000 |
|  4 | 淘宝店    |  1000 |
|  5 | c         |   100 |
+----+-----------+-------+
5 rows in set (0.00 sec)

2.张全蛋-成都
-- 手动开启事务
start transaction;

3.王尼玛-北京
-- 手动开启事务
start transaction;

4.张全蛋-成都
-- 插入数据
mysql> insert into user values(6,'d',1000);
Query OK, 1 row affected (0.00 sec)

mysql> select * from user;
+----+-----------+-------+
| id | name      | money |
+----+-----------+-------+
|  1 | a         |   800 |
|  2 | b         |  1200 |
|  3 | 小明      |  1000 |
|  4 | 淘宝店    |  1000 |
|  5 | c         |   100 |
|  6 | d         |  1000 |
+----+-----------+-------+
6 rows in set (0.00 sec)

5.王尼玛-北京

mysql> select * from user;
+----+-----------+-------+
| id | name      | money |
+----+-----------+-------+
|  1 | a         |   800 |
|  2 | b         |  1200 |
|  3 | 小明      |  1000 |
|  4 | 淘宝店    |  1000 |
|  5 | c         |   100 |
+----+-----------+-------+
5 rows in set (0.00 sec)

-- insert into user values(6,'d',1000);再次插入会出错

6.repeatable read总结
-- 这种现象叫做幻读！！
-- 事务a和事务b同时操作一张表，事务a提交的数据，也不能被事务b读到，就造成幻读。
```
- serializable(bank数据库 user表)
```
1.修改隔离级别
-- mysql 8.0
set global transaction isolation level serializable;

-- mysql 5.x（例子测试在全局隔离级别修改下)
mysql> set session transaction isolation level serializable;
Query OK, 0 rows affected (0.00 sec)

-- 修改后的隔离级别-串行化
mysql> select @@tx_isolation;
+----------------+
| @@tx_isolation |
+----------------+
| SERIALIZABLE   |
+----------------+
1 row in set, 1 warning (0.00 sec)

-- 查看数据
mysql> select * from user;
+----+-----------+-------+
| id | name      | money |
+----+-----------+-------+
|  1 | a         |   800 |
|  2 | b         |  1200 |
|  3 | 小明      |  1000 |
|  4 | 淘宝店    |  1000 |
|  5 | c         |   100 |
|  6 | d         |  1000 |
+----+-----------+-------+
6 rows in set (0.00 sec)

2.张全蛋-成都
-- 手动开启事务
start transaction;

3.王尼玛-北京
-- 手动开启事务
start transaction;

4. 张全蛋-成都
-- 插入数据
mysql> insert into user values(7,'赵铁柱',1000);
Query OK, 1 row affected (0.00 sec)

-- 手动提交 
mysql> commit;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from user;
+----+-----------+-------+
| id | name      | money |
+----+-----------+-------+
|  1 | a         |   800 |
|  2 | b         |  1200 |
|  3 | 小明      |  1000 |
|  4 | 淘宝店    |  1000 |
|  5 | c         |   100 |
|  6 | d         |  1000 |
|  7 | 赵铁柱    |  1000 |
+----+-----------+-------+
7 rows in set (0.00 sec)

5.王尼玛-北京
-- 查看数据
mysql> select * from user;
+----+-----------+-------+
| id | name      | money |
+----+-----------+-------+
|  1 | a         |   800 |
|  2 | b         |  1200 |
|  3 | 小明      |  1000 |
|  4 | 淘宝店    |  1000 |
|  5 | c         |   100 |
|  6 | d         |  1000 |
|  7 | 赵铁柱    |  1000 |
+----+-----------+-------+
7 rows in set (0.00 sec)

6.张全蛋-成都
-- 插入数据
insert into user values(8,'王小花',1000);

注意：sql语句被卡住（现在操作是串行化，当一边开启一个事物对user表进行操作，另外一边不可以进行user表写入，等待另一边commit）

7.王尼玛-北京
-- 手动提交
commit;

8.张全蛋-成都(插入才成功)
mysql> insert into user values(8,'王小花',1000);
Query OK, 1 row affected (0.00 sec)

mysql> select * from user;
+----+-----------+-------+
| id | name      | money |
+----+-----------+-------+
|  1 | a         |   800 |
|  2 | b         |  1200 |
|  3 | 小明      |  1000 |
|  4 | 淘宝店    |  1000 |
|  5 | c         |   100 |
|  6 | d         |  1000 |
|  7 | 赵铁柱    |  1000 |
|  8 | 王小花    |  1000 |
+----+-----------+-------+
8 rows in set (0.00 sec)

9.serializable总结
-- 当user表被另外事务操作，其他事物里面的写操作，是不可以进行。
-- 进入排队状态，知道王尼玛提交事务结束之后，张全蛋这个写入操作才会执行。
-- 在没有等待超时的情况下。
```
- 事务隔离性小结
```
-- mysql默认隔离级别是repeatable read
-- 隔离级别越高，性能越差
read uncommitted > read committed > repeatable read > serializable;
```
