# 第四讲 MySQL数据库的三大设计范式
## 第一范式
- 数据表的所有字段都是不可分割的原子值。  
1. 创建数据表
```
mysql> create table student2(
    -> id int primary key,
    -> name varchar(20),
    -> address varchar(30)
    -> );
Query OK, 0 rows affected (0.02 sec)
```
2. 更改字符集
```
mysql> alter table student2 change name name varchar(20) character set utf8;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> alter table student2 change address address varchar(30) character set utf8;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0
```
3. 插入数据
```
mysql> insert into student2 values(1,'张三','中国四川省成都市武侯区武侯大道100号');
Query OK, 1 row affected (0.00 sec)

mysql> insert into student2 values(2,'李四','中国四川省成都市武侯区京城大道200号');
Query OK, 1 row affected (0.00 sec)

mysql> insert into student2 values(3,'王五','中国四川省成都市高新区天府大道99号');
Query OK, 1 row affected (0.00 sec)


mysql> select * from student2;
+----+--------+-----------------------------------------------------+
| id | name   | address                                             |
+----+--------+-----------------------------------------------------+
|  1 | 张三   | 中国四川省成都市武侯区武侯大道100号                 |
|  2 | 李四   | 中国四川省成都市武侯区京城大道200号                 |
|  3 | 王五   | 中国四川省成都市高新区天府大道99号                  |
+----+--------+-----------------------------------------------------+
3 rows in set (0.00 sec)
```
4. 上面字段值还可以继续拆分，就不满足第一范式  
5. 继续创建表
```
mysql> create table student3(
    -> id int primary key,
    -> name varchar(20),
    -> country varchar(30),
    -> privence varchar(30),
    -> city varchar(30),
    -> details varchar(30)
    -> );
Query OK, 0 rows affected (0.01 sec)

mysql> desc student3;
+----------+-------------+------+-----+---------+-------+
| Field    | Type        | Null | Key | Default | Extra |
+----------+-------------+------+-----+---------+-------+
| id       | int(11)     | NO   | PRI | NULL    |       |
| name     | varchar(20) | YES  |     | NULL    |       |
| country  | varchar(30) | YES  |     | NULL    |       |
| privence | varchar(30) | YES  |     | NULL    |       |
| city     | varchar(30) | YES  |     | NULL    |       |
| details  | varchar(30) | YES  |     | NULL    |       |
+----------+-------------+------+-----+---------+-------+
6 rows in set (0.01 sec)
```
6. 更改字符集
```
mysql> alter table student3 change name name varchar(20) character set utf8;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> alter table student3 change country country varchar(30) character set utf8;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> alter table student3 change privence privence varchar(30) character set utf8;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> alter table student3 change city city varchar(30) character set utf8;
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> alter table student3 change details details varchar(30) character set utf8;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0
```
7. 插入数据(假设最后字段不可拆分)--满足第一范式
```
mysql> insert into student3 values(1,'张三','中国','四川省','成都市','武侯区武侯大道100号');
Query OK, 1 row affected (0.00 sec)

mysql> insert into student3 values(2,'李四','中国','四川省','成都市','武侯区京城大道200号');
Query OK, 1 row affected (0.00 sec)

mysql> insert into student3 values(3,'王五','中国','四川省','成都市','高新区天府大道99号');
Query OK, 1 row affected (0.00 sec)

mysql> select * from student3;
+----+--------+---------+-----------+-----------+-----------------------------+
| id | name   | country | privence  | city      | details                     |
+----+--------+---------+-----------+-----------+-----------------------------+
|  1 | 张三   | 中国    | 四川省    | 成都市    | 武侯区武侯大道100号         |
|  2 | 李四   | 中国    | 四川省    | 成都市    | 武侯区京城大道200号         |
|  3 | 王五   | 中国    | 四川省    | 成都市    | 高新区天府大道99号          |
+----+--------+---------+-----------+-----------+-----------------------------+
3 rows in set (0.00 sec)
```
8. 范式，设计得越详细，对于某些实际操作可能更好，但是不一定都是好处。主要以实际开发的便利为主。
## 第二范式
- 必须是满足第一范式的前提下，第二范式要求，除主键外的每一列都必须完全依赖于主键。如果要出现不完全依赖，只可能发生在联合主键的情况下。  
1. 不满足第二范式——>满足第二范式(拆表)
```
-- 订单表
create table myorder(
	order_id int primary key,
	product_id int,
	customer_id int,
);

-- 产品表
create table product(
	id int primary key
	name varchar(20),
	foreian key(id) references myorder(product_id)
);

-- 消费者表
create table customer(
	id int primary key,
	name varchar(20),
	foreian key(id) references myorder(customer_id)
);
```
2. 分成三个表，满足第二范式的设计！！
## 第三范式
- 必须先满足第二范式，除开主键列的其他列之间不能有传递依赖关系。
1. 订单表(设计不满足第三范式)
```
create table myorder(
	order_id int primary key,
	product_id int,
	customer_id int,
	customer_phone varchar(15)
);
```
2. 消费者表(满足第三范式的设计)
```
create table customer(
	id int primary key,
	name varchar(20),
	phone varchar(15),
	foreian key(id) references myorder(customer_id)
);
```

