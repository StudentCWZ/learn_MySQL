# 第六讲 MySQL的连接查询
## MySQL的连接概念
- 内连接
```
inner join 或者 join
```
- 外连接
```
1.左连接 left join 或者 left outer join
2.右连接 right join 或者 right outer join
3.完全外连接 full join 或者 full outer join 
```
## 连接查询的实践准备
1.创建数据库
```
mysql> create database testJoin;
Query OK, 1 row affected (0.00 sec)
```
2.创建两个表
```
-- person 表
id,name,cardId

mysql> create table person(id int,name varchar(20),cardId int);
Query OK, 0 rows affected (0.02 sec)

-- card 表
id,name

mysql> create table card(id int,name varchar(20));
Query OK, 0 rows affected (0.01 sec)
```
3.往card表插入数据
```
-- 更改字符集
mysql> alter table  card change name name varchar(20) character set utf8;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

-- 插入数据
insert into card values(1,'饭卡');
insert into card values(2,'建行卡');
insert into card values(3,'农行卡');
insert into card values(4,'工商卡');
insert into card values(5,'邮政卡');

-- 插入完成
mysql> select * from card;
+------+-----------+
| id   | name      |
+------+-----------+
|    1 | 饭卡      |
|    2 | 建行卡    |
|    3 | 农行卡    |
|    4 | 工商卡    |
|    5 | 邮政卡    |
+------+-----------+
5 rows in set (0.00 sec)
```
4.往person表插入数据
```
-- 更改字符集
alter table  person change name name varchar(20) character set utf8;
mysql> alter table  person change name name varchar(20) character set utf8;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0


-- 插入数据
insert into person values(1,'张三',1);
insert into person values(2,'李四',3);
insert into person values(3,'王五',6);

-- 插入完成
mysql> select * from person;
+------+--------+--------+
| id   | name   | cardId |
+------+--------+--------+
|    1 | 张三   |      1 |
|    2 | 李四   |      3 |
|    3 | 王五   |      6 |
+------+--------+--------+
3 rows in set (0.00 sec)
```
## MySQL的连接查询
1.inner join 查询/join 效果一样(内连接)
内连接查询，其实就是两张表中的数据，通过某个字段相等，查询出相关记录数据。
```
mysql> select * from person inner join card on person.cardId=card.id;
+------+--------+--------+------+-----------+
| id   | name   | cardId | id   | name      |
+------+--------+--------+------+-----------+
|    1 | 张三   |      1 |    1 | 饭卡      |
|    2 | 李四   |      3 |    3 | 农行卡    |
+------+--------+--------+------+-----------+
2 rows in set (0.01 sec)
```
2.left join /left outer join (左外连接)
左外连接，会把左边表里面的所有数据取出来，而右边表中的数据，如果有相等，就显示出来，如果没有，就会补NULL
```
mysql> select * from person left outer join card on person.cardId=card.id;
+------+--------+--------+------+-----------+
| id   | name   | cardId | id   | name      |
+------+--------+--------+------+-----------+
|    1 | 张三   |      1 |    1 | 饭卡      |
|    2 | 李四   |      3 |    3 | 农行卡    |
|    3 | 王五   |      6 | NULL | NULL      |
+------+--------+--------+------+-----------+
3 rows in set (0.00 sec)
```
3.right join /right outer join(右外连接)
右外连接，会把右边表里面的所有数据取出来，而左边表中的数据，如果有相等，就显示出来，如果没有，就会补NULL
```
mysql> select * from person right outer join card on person.cardId=card.id;
+------+--------+--------+------+-----------+
| id   | name   | cardId | id   | name      |
+------+--------+--------+------+-----------+
|    1 | 张三   |      1 |    1 | 饭卡      |
|    2 | 李四   |      3 |    3 | 农行卡    |
| NULL | NULL   |   NULL |    2 | 建行卡    |
| NULL | NULL   |   NULL |    4 | 工商卡    |
| NULL | NULL   |   NULL |    5 | 邮政卡    |
+------+--------+--------+------+-----------+
5 rows in set (0.00 sec)
```
4.full join(全外连接)
```
mysql> select * from person full join card on person.cardId=card.id;
ERROR 1054 (42S22): Unknown column 'person.cardId' in 'on clause'

-- mysql不支持 full join
mysql> select * from person left join card on person.cardId=card.id union select * from person right join card on person.cardId=card.id;
+------+--------+--------+------+-----------+
| id   | name   | cardId | id   | name      |
+------+--------+--------+------+-----------+
|    1 | 张三   |      1 |    1 | 饭卡      |
|    2 | 李四   |      3 |    3 | 农行卡    |
|    3 | 王五   |      6 | NULL | NULL      |
| NULL | NULL   |   NULL |    2 | 建行卡    |
| NULL | NULL   |   NULL |    4 | 工商卡    |
| NULL | NULL   |   NULL |    5 | 邮政卡    |
+------+--------+--------+------+-----------+
6 rows in set (0.00 sec)
```






