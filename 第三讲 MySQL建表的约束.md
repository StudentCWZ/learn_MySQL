# 第三讲 MySQL建表的约束
## MySQL建表的约束
#### 主键约束
它可以唯一确定一张表中的一条记录，也就是我们通过给某个字段添加约束，就可以使得该字段不重复且不为空。  
1.添加主键约束
```
mysql> use test;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> create table user(
    -> id int primary key,
    -> name varchar(20)
    -> );
Query OK, 0 rows affected (0.01 sec)

mysql> show tables;
+----------------+
| Tables_in_test |
+----------------+
| pet            |
| testType       |
| user           |
+----------------+
3 rows in set (0.00 sec)

mysql> describe user;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | NO   | PRI | NULL    |       |
| name  | varchar(20) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
```
2.更改字符集
```
mysql> alter table user change name name varchar(20) character set utf8;
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0
```
3.插入数据(成功)
```
mysql> insert into user values(1,'张三');
Query OK, 1 row affected (0.00 sec)
```
4.由于主键约束，添加数据id字段不可以重复且不为空
```
mysql> insert into user values(1,'张三');
ERROR 1062 (23000): Duplicate entry '1' for key 'PRIMARY'

mysql> insert into user values(2,'张三');
Query OK, 1 row affected (0.00 sec)

mysql> select * from user;
+----+--------+
| id | name   |
+----+--------+
|  1 | 张三   |
|  2 | 张三   |
+----+--------+

mysql> insert into user values(NULL,'张三');
ERROR 1048 (23000): Column 'id' cannot be null
```
#### 联合约束
只要联合的主键值加起来不重复就可以  
1.建立联合约束
```
mysql> create table user2(
    -> id int,
    -> name varchar(20),
    -> password varchar(20),
    -> primary key(id,name)
    -> );
Query OK, 0 rows affected (0.01 sec)
```
2.更改字符集
```
mysql> alter table user2 change name name varchar(20) character set utf8;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0
```
3.插入数据
```
mysql> insert into user2 values(1,'张三','123');
Query OK, 1 row affected (0.01 sec)
```
4.再插入相同数据报错
```
mysql> insert into user2 values(1,'张三','123');
ERROR 1062 (23000): Duplicate entry '1-张三' for key 'PRIMARY'
```
5.继续插入数据
```
mysql> insert into user2 values(2,'张三','123');
Query OK, 1 row affected (0.01 sec)

mysql> insert into user2 values(1,'李四','123');
Query OK, 1 row affected (0.00 sec)

mysql> select * from user2;
+----+--------+----------+
| id | name   | password |
+----+--------+----------+
|  1 | 张三   | 123      |
|  1 | 李四   | 123      |
|  2 | 张三   | 123      |
+----+--------+----------+
3 rows in set (0.00 sec)
```
6.联合主键中的每个主键字段都不能为空
```
mysql> insert into user2 values(NULL,'李四','123');
ERROR 1048 (23000): Column 'id' cannot be null
```
#### 自增约束
auto_increment(管控主键的值，并自动增加)  
1.添加自增约束
```
mysql> create table user3(
    -> id int primary key auto_increment,
    -> name varchar(20)
    -> );
Query OK, 0 rows affected (0.02 sec)
```
2.更改字符集
```
mysql> alter table user3 change name name varchar(20) character set utf8;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0
````
3.插入数据
```
mysql> insert into user3 (name) values('zhangsan');
Query OK, 1 row affected (0.00 sec)

mysql> select * from user3;
+----+----------+
| id | name     |
+----+----------+
|  1 | zhangsan |
+----+----------+
1 row in set (0.00 sec)
```
4.再次插入
```
mysql> insert into user3 (name) values('zhangsan');
Query OK, 1 row affected (0.00 sec)

mysql> select * from user3;
+----+----------+
| id | name     |
+----+----------+
|  1 | zhangsan |
|  2 | zhangsan |
+----+----------+
2 rows in set (0.00 sec)
```
5.创建表后忘记创建主键约束，进行主键添加(建表后添加主键约束)
```
mysql> create table user4(
    -> id int,
    -> name varchar(20)
    -> );
Query OK, 0 rows affected (0.02 sec)

mysql> desc user4;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  |     | NULL    |       |
| name  | varchar(20) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)

mysql> alter table user4 add primary key(id);
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc user4;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | NO   | PRI | NULL    |       |
| name  | varchar(20) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
```
6.删除主键
```
mysql> alter table user4 drop primary key;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc user4;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | NO   |     | NULL    |       |
| name  | varchar(20) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.01 sec)
```
7.修改一个字段来添加主键
```
mysql> alter table user4 modify id int primary key;
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc user4;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | NO   | PRI | NULL    |       |
| name  | varchar(20) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
```
#### 唯一约束
约束修饰的字段的值不可以重复  
1.添加唯一约束
```
mysql> create table user5(
    -> id int,
    -> name varchar(20)
    -> );
Query OK, 0 rows affected (0.02 sec)

mysql> alter table user5 add unique(name);
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc user5;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  |     | NULL    |       |
| name  | varchar(20) | YES  | UNI | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
```
2.更改字符集
```
mysql> alter table user5 change name name varchar(20) character set utf8;
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0
```
3.插入数据
```
mysql> insert into user5 values(1,'zhangsan');
Query OK, 1 row affected (0.00 sec)
```
4.name字段重复了(唯一约束)
```
mysql> insert into user5 values(1,'zhangsan');
ERROR 1062 (23000): Duplicate entry 'zhangsan' for key 'name'
```
5.正常插入
```
mysql> insert into user5 values(1,'lisi');
Query OK, 1 row affected (0.01 sec)
```
6.创建表时直接添加唯一约束
```
mysql> create table user6(
    -> id int,
    -> name varchar(20),
    -> unique(name)
    -> );
Query OK, 0 rows affected (0.01 sec)

mysql> desc user6;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  |     | NULL    |       |
| name  | varchar(20) | YES  | UNI | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
```
7.unique位置改变
```
mysql> create table user7(
    -> id int,
    -> name varchar(20) unique
    -> );
Query OK, 0 rows affected (0.01 sec)

mysql> desc user7;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  |     | NULL    |       |
| name  | varchar(20) | YES  | UNI | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
```
8.删除唯一约束
```
mysql> alter table user7 drop index name;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc user7;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  |     | NULL    |       |
| name  | varchar(20) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
```
9.modify方式添加唯一约束
```
mysql> alter table user7 modify name varchar(20) unique;
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc user7;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  |     | NULL    |       |
| name  | varchar(20) | YES  | UNI | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.01 sec)
```
#### 联合唯一约束
unique(id,name)表示两个键在一起不重复就行  
1.添加联合唯一约束
```
mysql> create table user8(
    -> id int,
    -> name varchar(20),
    -> unique(id,name)
    -> );
Query OK, 0 rows affected (0.02 sec)

mysql> desc user8;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  | MUL | NULL    |       |
| name  | varchar(20) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
```
2.更改字符集
```
mysql> alter table user8 change name name varchar(20) character set utf8;
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0
```
3.插入数据
```
mysql> insert into user8 values(1,'zhangsan');
Query OK, 1 row affected (0.01 sec)

mysql> insert into user8 values(1,'zhangsan');
ERROR 1062 (23000): Duplicate entry '1-zhangsan' for key 'id'
mysql> insert into user8 values(2,'zhangsan');
Query OK, 1 row affected (0.00 sec)

mysql> select * from user8;
+------+----------+
| id   | name     |
+------+----------+
|    1 | zhangsan |
|    2 | zhangsan |
+------+----------+
2 rows in set (0.00 sec)
```
4.再度插入数据
```
mysql> insert into user8 values(1,'lisi');
Query OK, 1 row affected (0.00 sec)

mysql> select * from user8;
+------+----------+
| id   | name     |
+------+----------+
|    1 | lisi     |
|    1 | zhangsan |
|    2 | zhangsan |
+------+----------+
3 rows in set (0.01 sec)
```
#### 非空约束
修饰字段不能为null  
1.添加非空约束
```
mysql> create table user9(
    -> id int,
    -> name varchar(20) not null
    -> );
Query OK, 0 rows affected (0.02 sec)

mysql> desc user9;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  |     | NULL    |       |
| name  | varchar(20) | NO   |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.01 sec)
```
2.更改字符集
```
mysql> alter table user9 change name name varchar(20) character set utf8;
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0
```
3.插入数据
```
mysql> insert into user9 (id) values(1); # 报错
```
4.再次插入数据
```
mysql> insert into user9 values(1,'张三');
Query OK, 1 row affected (0.00 sec)

mysql> select * from user9;
+------+--------+
| id   | name   |
+------+--------+
|    1 | 张三   |
+------+--------+
1 row in set (0.00 sec)
```
5.继续插入数据
```
mysql> insert into user9 (name) values('lisi');
Query OK, 1 row affected (0.01 sec)

mysql> select * from user9;
+------+--------+
| id   | name   |
+------+--------+
|    1 | 张三   |
| NULL | lisi   |
+------+--------+
2 rows in set (0.00 sec)
```
#### 默认约束
就是当我们插入字段值的时候，如果没有传值，就会使用默认值  
1.添加默认约束
```
mysql> create table user10(
    -> id int,
    -> name varchar(20),
    -> age int Default 10
    -> );
Query OK, 0 rows affected (0.01 sec)

mysql> desc user10;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  |     | NULL    |       |
| name  | varchar(20) | YES  |     | NULL    |       |
| age   | int(11)     | YES  |     | 10      |       |
+-------+-------------+------+-----+---------+-------+
3 rows in set (0.00 sec)
```
2.插入数据
```
mysql> insert into user10 (id,name) values(1,'zhangsan');
Query OK, 1 row affected (0.00 sec)

mysql> select * from user10;
+------+----------+------+
| id   | name     | age  |
+------+----------+------+
|    1 | zhangsan |   10 |
+------+----------+------+
1 row in set (0.01 sec)
```
3.再插入数据(传了值，就不会使用默认值)
```
mysql> insert into user10 values(1,'zhangsan',19);
Query OK, 1 row affected (0.01 sec)

mysql> select * from user10;
+------+----------+------+
| id   | name     | age  |
+------+----------+------+
|    1 | zhangsan |   10 |
|    1 | zhangsan |   19 |
+------+----------+------+
2 rows in set (0.00 sec)
```
#### 外键约束
涉及到两个表：父表、子表
主表、副表  
1.建立外键约束
```
-- 班级表
mysql> create table classes(
    -> id int primary key,
    -> name varchar(20)
    -> );
Query OK, 0 rows affected (0.01 sec)

mysql> alter table classes change name name varchar(20) character set utf8;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc classes;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | NO   | PRI | NULL    |       |
| name  | varchar(20) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)

-- 学生表
mysql> create table students(
    -> id int primary key,
    -> name varchar(20),
    -> class_id int,
    -> foreign key(class_id) references classes(id)
    -> );
Query OK, 0 rows affected (0.01 sec)

mysql> alter table students change name name varchar(20) character set utf8;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc students;
+----------+-------------+------+-----+---------+-------+
| Field    | Type        | Null | Key | Default | Extra |
+----------+-------------+------+-----+---------+-------+
| id       | int(11)     | NO   | PRI | NULL    |       |
| name     | varchar(20) | YES  |     | NULL    |       |
| class_id | int(11)     | YES  | MUL | NULL    |       |
+----------+-------------+------+-----+---------+-------+
3 rows in set (0.00 sec)
```
2.classes插入数据
```
mysql> insert into classes values(1,'一班');
Query OK, 1 row affected (0.00 sec)

mysql> insert into classes values(2,'二班');
Query OK, 1 row affected (0.00 sec)

mysql> insert into classes values(3,'三班');
Query OK, 1 row affected (0.00 sec)

mysql> insert into classes values(4,'四班');
Query OK, 1 row affected (0.00 sec)

mysql> select * from classes;
+----+--------+
| id | name   |
+----+--------+
|  1 | 一班   |
|  2 | 二班   |
|  3 | 三班   |
|  4 | 四班   |
+----+--------+
4 rows in set (0.00 sec)
```
3.往students表中插入数据
```
mysql> insert into students values(1001,'张三',1);
Query OK, 1 row affected (0.00 sec)

mysql> insert into students values(1002,'张三',2);
Query OK, 1 row affected (0.00 sec)

mysql> insert into students values(1003,'张三',3);
Query OK, 1 row affected (0.00 sec)

mysql> insert into students values(1004,'张三',4);
Query OK, 1 row affected (0.00 sec)

mysql> select * from students;
+------+--------+----------+
| id   | name   | class_id |
+------+--------+----------+
| 1001 | 张三   |        1 |
| 1002 | 张三   |        2 |
| 1003 | 张三   |        3 |
| 1004 | 张三   |        4 |
+------+--------+----------+
4 rows in set (0.00 sec)
```
4.再插入数据(报错)
```
mysql> insert into students values(1005,'lisi',5);
ERROR 1452 (23000): Cannot add or update a child row: a foreign key constraint fails (`test`.`students`, CONSTRAINT `students_ibfk_1` FOREIGN KEY (`class_id`) REFERENCES `classes` (`id`))
```
5.主表中的记录被副表引用，是不可以被删除的
```
mysql> delete from classes where id=4;
ERROR 1451 (23000): Cannot delete or update a parent row: a foreign key constraint fails (`test`.`students`, CONSTRAINT `students_ibfk_1` FOREIGN KEY (`class_id`) REFERENCES `classes` (`id`))

```
#### 本讲小结：
- 1.建表的时候添加约束(主键约束、联合约束、唯一约束、联合唯一约束、自增约束)
- 2.可以使用alter...add...(主键约束、联合约束、唯一约束、联合唯一约束、自增约束)
- 3.alter...modify...(主键约束、联合约束、唯一约束、联合唯一约束、自增约束)
- 4.删除alter...drop...(主键约束、联合约束、唯一约束、联合唯一约束、自增约束)
- 5.主表classes中没有的数据值，在副表中是不可以使用的。(外键约束中内容)
- 6.主表中的记录被副表引用，是不可以被删除的。(外键约束中内容)
