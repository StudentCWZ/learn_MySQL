# 第五讲 MySQL的课堂练习
## 数据库中修改字符集手段
1. 修改字符集
```
SET character_set_client = utf8;
SET character_set_connection = utf8;
SET character_set_database = utf8;
SET character_set_results = utf8;
SET character_set_server = utf8;
SET collation_connection = utf8;
SET collation_database = utf8;
SET collation_server = utf8;
```
2. 修改数据库的字符集
```
alter database selectTest character set utf8;
```
3. 修改数据表的字符集
```
alter table student character set utf8;
```
4. 修改数据表字段的字符集
```
alter table student change student_name student_name varchar(10) character set utf8;
```
## MySQL的查询练习(学生表、课程表、教师表、成绩表)
1. 建立学生表
```
create table student(
	student_id varchar(20) primary key,
	student_name varchar(20) not null,
	student_sex varchar(10) not null,
	student_birthday datetime,
	class varchar(20)
);
```
2. 修改数据表字段的字符集
```
alter table student change student_name student_name varchar(20) character set utf8;
alter table student change student_sex student_sex varchar(10) character set utf8;
```
3. 建立课程表
```
create table course(
	course_id varchar(20) primary key,
	course_name varchar(20) not null,
	teacher_id varchar(20) not null,
	foreign key(teacher_id) references teacher(teacher_id)
);
```
4. 修改数据表字段的字符集
```
alter table course change course_name course_name varchar(20) character set utf8;
```
5. 建立教师表
```
create table teacher(
	teacher_id varchar(20) primary key,
	teacher_name varchar(20) not null,
	teacher_sex varchar(10) not null,
	teacher_birthday datetime,
	teacher_profession varchar(20) not null,
	teacher_department varchar(20) not null
);
```
6. 修改数据表字段的字符集
```
alter table teacher change teacher_name teacher_name varchar(20) character set utf8;
alter table teacher change teacher_sex teacher_sex varchar(10) character set utf8;
alter table teacher change teacher_profession teacher_profession varchar(20) character set utf8;
alter table teacher change teacher_department teacher_department varchar(20) character set utf8;
```
7. 建立成绩表
```
# 修改前
create table score(
	student_id varchar(20) primary key,
	course_id varchar(20) not null,
	degree decimal,
	foreign key(student_id) references student(student_id),
	foreign key(course_id) references course(course_id)
);

# 修改后
create table score(
	student_id varchar(20) NOT NULL,
	course_id varchar(20) NOT NULL,
	degree decimal,
	foreign key(student_id) references student(student_id),
	foreign key(course_id) references course(course_id),
	primary key(student_id, course_id)
);
```
8. 往数据表中添加数据
```
# 添加学生信息
insert into student values('101','管华','男','1977-09-01','95033');
insert into student values('102','匡明','男','1975-10-02','95031');
insert into student values('103','王丽','女','1976-01-23','95033');
insert into student values('104','李军','男','1976-02-20','95033');
insert into student values('105','王芳','女','1975-02-10','95031');
insert into student values('106','陆军','男','1974-06-03','95031');
insert into student values('107','王尼玛','男','1976-02-20','95033');
insert into student values('108','张全蛋','男','1975-02-10','95031');
insert into student values('109','赵铁柱','男','1974-06-03','95031');



# 添加教师信息
insert into teacher values('804','李城','男','1958-12-02','副教授','计算机系');
insert into teacher values('856','张旭','男','1969-03-12','讲师','电子工程系');
insert into teacher values('825','王萍','女','1972-05-05','助教','计算机系');
insert into teacher values('831','刘冰','女','1977-08-14','助教','电子工程系');


# 添加课程信息
insert into course values('3-105','计算机导论','825');
insert into course values('3-245','操作系统','804');
insert into course values('6-166','数字电路','856');
insert into course values('9-888','高等数学','831');


# 添加成绩表
insert into score values('103','3-245','86');
insert into score values('105','3-245','75');
insert into score values('109','3-245','68');
insert into score values('103','3-105','92');
insert into score values('105','3-105','88');
insert into score values('109','3-105','76');
insert into score values('103','6-166','85');
insert into score values('105','6-166','79');
insert into score values('109','6-166','81');
```
9. 查询student表的所有记录
```
mysql> select * from student;
+------------+--------------+-------------+---------------------+-------+
| student_id | student_name | student_sex | student_birthday    | class |
+------------+--------------+-------------+---------------------+-------+
| 101        | 管华         | 男          | 1977-09-01 00:00:00 | 95033 |
| 102        | 匡明         | 男          | 1975-10-02 00:00:00 | 95031 |
| 103        | 王丽         | 女          | 1976-01-23 00:00:00 | 95033 |
| 104        | 李军         | 男          | 1976-02-20 00:00:00 | 95033 |
| 105        | 王芳         | 女          | 1975-02-10 00:00:00 | 95031 |
| 106        | 陆军         | 男          | 1974-06-03 00:00:00 | 95031 |
| 107        | 王尼玛       | 男          | 1976-02-20 00:00:00 | 95033 |
| 108        | 张全蛋       | 男          | 1975-02-10 00:00:00 | 95031 |
| 109        | 赵铁柱       | 男          | 1974-06-03 00:00:00 | 95031 |
+------------+--------------+-------------+---------------------+-------+
9 rows in set (0.00 sec)
```
10. 查询student表中的所有记录的student_name、student_sex和class列
```
mysql> select student_name,student_sex,class from student;
+--------------+-------------+-------+
| student_name | student_sex | class |
+--------------+-------------+-------+
| 管华         | 男          | 95033 |
| 匡明         | 男          | 95031 |
| 王丽         | 女          | 95033 |
| 李军         | 男          | 95033 |
| 王芳         | 女          | 95031 |
| 陆军         | 男          | 95031 |
| 王尼玛       | 男          | 95033 |
| 张全蛋       | 男          | 95031 |
| 赵铁柱       | 男          | 95031 |
+--------------+-------------+-------+
9 rows in set (0.00 sec)
```
11. 查询教师所有的单位即不重复的teacher_department列
```
-- distinct排除重复
mysql> select distinct teacher_department from teacher;
+--------------------+
| teacher_department |
+--------------------+
| 计算机系           |
| 电子工程系         |
+--------------------+
2 rows in set (0.00 sec)
```
12. 查询score表中成绩在60到80之间的所有记录
```
# 查询区间 between ... and ...
mysql> select * from score where degree between 60 and 80;
+------------+-----------+--------+
| student_id | course_id | degree |
+------------+-----------+--------+
| 105        | 3-245     |     75 |
| 105        | 6-166     |     79 |
| 109        | 3-105     |     76 |
| 109        | 3-245     |     68 |
+------------+-----------+--------+
4 rows in set (0.00 sec)

# 运算符比较
mysql> select * from score where degree > 60 and degree < 80;
+------------+-----------+--------+
| student_id | course_id | degree |
+------------+-----------+--------+
| 105        | 3-245     |     75 |
| 105        | 6-166     |     79 |
| 109        | 3-105     |     76 |
| 109        | 3-245     |     68 |
+------------+-----------+--------+
4 rows in set (0.00 sec)
```
13. 查询score表中成绩为85，86或88的记录
```
-- 表示或者关系的查询(in)
mysql> select * from score where degree in (85, 86, 88);
+------------+-----------+--------+
| student_id | course_id | degree |
+------------+-----------+--------+
| 103        | 3-245     |     86 |
| 103        | 6-166     |     85 |
| 105        | 3-105     |     88 |
+------------+-----------+--------+
3 rows in set (0.00 sec)
```
14. 查询student表中“95031”班或性别为“女”的同学记录
```
-- or表示或者
mysql> select * from student where class='95031' or student_sex='女';
+------------+--------------+-------------+---------------------+-------+
| student_id | student_name | student_sex | student_birthday    | class |
+------------+--------------+-------------+---------------------+-------+
| 102        | 匡明         | 男          | 1975-10-02 00:00:00 | 95031 |
| 103        | 王丽         | 女          | 1976-01-23 00:00:00 | 95033 |
| 105        | 王芳         | 女          | 1975-02-10 00:00:00 | 95031 |
| 106        | 陆军         | 男          | 1974-06-03 00:00:00 | 95031 |
| 108        | 张全蛋       | 男          | 1975-02-10 00:00:00 | 95031 |
| 109        | 赵铁柱       | 男          | 1974-06-03 00:00:00 | 95031 |
+------------+--------------+-------------+---------------------+-------+
6 rows in set (0.00 sec)
```
15. 以class顺序查询student表的所有记录
```
# 降序（默认升序）
mysql> select * from student order by class desc;
+------------+--------------+-------------+---------------------+-------+
| student_id | student_name | student_sex | student_birthday    | class |
+------------+--------------+-------------+---------------------+-------+
| 101        | 管华         | 男          | 1977-09-01 00:00:00 | 95033 |
| 103        | 王丽         | 女          | 1976-01-23 00:00:00 | 95033 |
| 104        | 李军         | 男          | 1976-02-20 00:00:00 | 95033 |
| 107        | 王尼玛       | 男          | 1976-02-20 00:00:00 | 95033 |
| 102        | 匡明         | 男          | 1975-10-02 00:00:00 | 95031 |
| 105        | 王芳         | 女          | 1975-02-10 00:00:00 | 95031 |
| 106        | 陆军         | 男          | 1974-06-03 00:00:00 | 95031 |
| 108        | 张全蛋       | 男          | 1975-02-10 00:00:00 | 95031 |
| 109        | 赵铁柱       | 男          | 1974-06-03 00:00:00 | 95031 |
+------------+--------------+-------------+---------------------+-------+
9 rows in set (0.01 sec)

# 升序
mysql> select * from student order by class asc;
+------------+--------------+-------------+---------------------+-------+
| student_id | student_name | student_sex | student_birthday    | class |
+------------+--------------+-------------+---------------------+-------+
| 102        | 匡明         | 男          | 1975-10-02 00:00:00 | 95031 |
| 105        | 王芳         | 女          | 1975-02-10 00:00:00 | 95031 |
| 106        | 陆军         | 男          | 1974-06-03 00:00:00 | 95031 |
| 108        | 张全蛋       | 男          | 1975-02-10 00:00:00 | 95031 |
| 109        | 赵铁柱       | 男          | 1974-06-03 00:00:00 | 95031 |
| 101        | 管华         | 男          | 1977-09-01 00:00:00 | 95033 |
| 103        | 王丽         | 女          | 1976-01-23 00:00:00 | 95033 |
| 104        | 李军         | 男          | 1976-02-20 00:00:00 | 95033 |
| 107        | 王尼玛       | 男          | 1976-02-20 00:00:00 | 95033 |
+------------+--------------+-------------+---------------------+-------+
9 rows in set (0.00 sec)
```
16. 以course_id升序、degree降序查询score表的所有记录
```
mysql> select * from score order by course_id asc,degree desc;
+------------+-----------+--------+
| student_id | course_id | degree |
+------------+-----------+--------+
| 103        | 3-105     |     92 |
| 105        | 3-105     |     88 |
| 109        | 3-105     |     76 |
| 103        | 3-245     |     86 |
| 105        | 3-245     |     75 |
| 109        | 3-245     |     68 |
| 103        | 6-166     |     85 |
| 109        | 6-166     |     81 |
| 105        | 6-166     |     79 |
+------------+-----------+--------+
9 rows in set (0.00 sec)
```
17. 查询“95031”班的学生人数
```
# 统计count
mysql> select count(*) from student where class='95031';
+----------+
| count(*) |
+----------+
|        5 |
+----------+
1 row in set (0.01 sec)
```
18. 查询score表中的最高分的学生学号和课程号(子查询或者排序)
```
# 子查询做法
1.找到最高分
select max(degree) from score;

2.找到最高分对应的student_id和course_id
select student_id,course_id from score where degree=(select max(degree) from score);

mysql> select student_id,course_id from score where degree=(select max(degree) from score);
+------------+-----------+
| student_id | course_id |
+------------+-----------+
| 103        | 3-105     |
+------------+-----------+
1 row in set (0.00 sec)

# 排序的做法
mysql> select student_id,course_id,degree from score order by degree asc;
+------------+-----------+--------+
| student_id | course_id | degree |
+------------+-----------+--------+
| 109        | 3-245     |     68 |
| 105        | 3-245     |     75 |
| 109        | 3-105     |     76 |
| 105        | 6-166     |     79 |
| 109        | 6-166     |     81 |
| 103        | 6-166     |     85 |
| 103        | 3-245     |     86 |
| 105        | 3-105     |     88 |
| 103        | 3-105     |     92 |
+------------+-----------+--------+
9 rows in set (0.01 sec)

-- limit第一个数字表示从多少开始
-- limit第二个数字表示查多少条
mysql> select student_id,course_id,degree from score order by degree desc limit 0,1;
+------------+-----------+--------+
| student_id | course_id | degree |
+------------+-----------+--------+
| 103        | 3-105     |     92 |
+------------+-----------+--------+
1 row in set (0.00 sec)
```
19. 查询每门课的平均成绩
```
mysql> select * from course;
+-----------+-----------------+------------+
| course_id | course_name     | teacher_id |
+-----------+-----------------+------------+
| 3-105     | 计算机导论      | 825        |
| 3-245     | 操作系统        | 804        |
| 6-166     | 数字电路        | 856        |
| 9-888     | 高等数学        | 831        |
+-----------+-----------------+------------+
4 rows in set (0.00 sec)
-- avg()表示平均
mysql> select avg(degree) from score where course_id ='3-105';
+-------------+
| avg(degree) |
+-------------+
|     85.3333 |
+-------------+
1 row in set (0.00 sec)

-- group by 分组
mysql> select course_id,avg(degree) from score group by course_id;
+-----------+-------------+
| course_id | avg(degree) |
+-----------+-------------+
| 3-105     |     85.3333 |
| 3-245     |     76.3333 |
| 6-166     |     81.6667 |
+-----------+-------------+
3 rows in set (0.00 sec)
```
20. 查询score表中至少有两名学生选修的并以3开头的课程的平均分数
```
-- 第一步
mysql> select course_id from score group by course_id;
+-----------+
| course_id |
+-----------+
| 3-105     |
| 3-245     |
| 6-166     |
+-----------+
3 rows in set (0.00 sec)
-- 第二步
mysql> select course_id from score group by course_id having count(course_id)>=2;
+-----------+
| course_id |
+-----------+
| 3-105     |
| 3-245     |
| 6-166     |
+-----------+
3 rows in set (0.00 sec)
-- 第三步
mysql> select course_id from score group by course_id having count(course_id) >=2 and course_id like '3%';
+-----------+
| course_id |
+-----------+
| 3-105     |
| 3-245     |
+-----------+
2 rows in set (0.00 sec)
-- 第四步
mysql> select course_id,avg(degree) from score group by course_id having count(course_id) >=2 and course_id like '3%';
+-----------+-------------+
| course_id | avg(degree) |
+-----------+-------------+
| 3-105     |     85.3333 |
| 3-245     |     76.3333 |
+-----------+-------------+
2 rows in set (0.01 sec)

-- 最终答案
mysql> select course_id,avg(degree),count(*) from score group by course_id having count(course_id) >=2 and course_id like '3%';
+-----------+-------------+----------+
| course_id | avg(degree) | count(*) |
+-----------+-------------+----------+
| 3-105     |     85.3333 |        3 |
| 3-245     |     76.3333 |        3 |
+-----------+-------------+----------+
2 rows in set (0.00 sec)
```
21. 查询分数大于70、小于90的student_id列
```
-- 第一种
mysql> select student_id,degree from score where 70 < degree < 90;
+------------+--------+
| student_id | degree |
+------------+--------+
| 103        |     92 |
| 103        |     86 |
| 103        |     85 |
| 105        |     88 |
| 105        |     75 |
| 105        |     79 |
| 109        |     76 |
| 109        |     68 |
| 109        |     81 |
+------------+--------+
9 rows in set (0.00 sec)

-- 第二种
mysql> select student_id,degree from score where degree > 70 and degree < 90;
+------------+--------+
| student_id | degree |
+------------+--------+
| 103        |     86 |
| 103        |     85 |
| 105        |     88 |
| 105        |     75 |
| 105        |     79 |
| 109        |     76 |
| 109        |     81 |
+------------+--------+
7 rows in set (0.00 sec)

-- 第三种
mysql> select student_id,degree from score where degree between 70 and 90;
+------------+--------+
| student_id | degree |
+------------+--------+
| 103        |     86 |
| 103        |     85 |
| 105        |     88 |
| 105        |     75 |
| 105        |     79 |
| 109        |     76 |
| 109        |     81 |
+------------+--------+
7 rows in set (0.01 sec)
```
22. 查询所有学生的student_name、course_id和degree列
```
-- 第一步
mysql> select student_name from student;
+--------------+
| student_name |
+--------------+
| 管华         |
| 匡明         |
| 王丽         |
| 李军         |
| 王芳         |
| 陆军         |
| 王尼玛       |
| 张全蛋       |
| 赵铁柱       |
+--------------+
9 rows in set (0.00 sec)

-- 第二步
mysql> select course_id, degree from score;
+-----------+--------+
| course_id | degree |
+-----------+--------+
| 3-105     |     92 |
| 3-245     |     86 |
| 6-166     |     85 |
| 3-105     |     88 |
| 3-245     |     75 |
| 6-166     |     79 |
| 3-105     |     76 |
| 3-245     |     68 |
| 6-166     |     81 |
+-----------+--------+
9 rows in set (0.00 sec)

-- 第三步
mysql> select student_id,student_name from student;
+------------+--------------+
| student_id | student_name |
+------------+--------------+
| 101        | 管华         |
| 102        | 匡明         |
| 103        | 王丽         |
| 104        | 李军         |
| 105        | 王芳         |
| 106        | 陆军         |
| 107        | 王尼玛       |
| 108        | 张全蛋       |
| 109        | 赵铁柱       |
+------------+--------------+
9 rows in set (0.00 sec)

-- 第四步
mysql> select student_id, course_id, degree from score;
+------------+-----------+--------+
| student_id | course_id | degree |
+------------+-----------+--------+
| 103        | 3-105     |     92 |
| 103        | 3-245     |     86 |
| 103        | 6-166     |     85 |
| 105        | 3-105     |     88 |
| 105        | 3-245     |     75 |
| 105        | 6-166     |     79 |
| 109        | 3-105     |     76 |
| 109        | 3-245     |     68 |
| 109        | 6-166     |     81 |
+------------+-----------+--------+
9 rows in set (0.01 sec)

-- 最终答案
mysql> select student_name,course_id,degree from student,score where student.student_id=score.student_id;
+--------------+-----------+--------+
| student_name | course_id | degree |
+--------------+-----------+--------+
| 王丽         | 3-105     |     92 |
| 王丽         | 3-245     |     86 |
| 王丽         | 6-166     |     85 |
| 王芳         | 3-105     |     88 |
| 王芳         | 3-245     |     75 |
| 王芳         | 6-166     |     79 |
| 赵铁柱       | 3-105     |     76 |
| 赵铁柱       | 3-245     |     68 |
| 赵铁柱       | 6-166     |     81 |
+--------------+-----------+--------+
9 rows in set (0.01 sec)
```
23. 查询所有学生的student_id、course_name和degree列
```
mysql> select student_id,course_name,degree from course,score where course.course_id=score.course_id;
+------------+-----------------+--------+
| student_id | course_name     | degree |
+------------+-----------------+--------+
| 103        | 计算机导论      |     92 |
| 103        | 操作系统        |     86 |
| 103        | 数字电路        |     85 |
| 105        | 计算机导论      |     88 |
| 105        | 操作系统        |     75 |
| 105        | 数字电路        |     79 |
| 109        | 计算机导论      |     76 |
| 109        | 操作系统        |     68 |
| 109        | 数字电路        |     81 |
+------------+-----------------+--------+
9 rows in set (0.00 sec)
```
24. 查询所有学生的student_name、course_name和degree列
```
mysql> select student_name,course_name,degree from student,course,score where student.student_id=score.student_id and course.course_id=score.course_id;
+--------------+-----------------+--------+
| student_name | course_name     | degree |
+--------------+-----------------+--------+
| 王丽         | 计算机导论      |     92 |
| 王丽         | 操作系统        |     86 |
| 王丽         | 数字电路        |     85 |
| 王芳         | 计算机导论      |     88 |
| 王芳         | 操作系统        |     75 |
| 王芳         | 数字电路        |     79 |
| 赵铁柱       | 计算机导论      |     76 |
| 赵铁柱       | 操作系统        |     68 |
| 赵铁柱       | 数字电路        |     81 |
+--------------+-----------------+--------+
9 rows in set (0.00 sec)
-- 添加查询字段
mysql> select student.student_id,course.course_id,student_name,course_name,degree from student,course,score where student.student_id=score.student_id and course.course_id=score.course_id;
+------------+-----------+--------------+-----------------+--------+
| student_id | course_id | student_name | course_name     | degree |
+------------+-----------+--------------+-----------------+--------+
| 103        | 3-105     | 王丽         | 计算机导论      |     92 |
| 103        | 3-245     | 王丽         | 操作系统        |     86 |
| 103        | 6-166     | 王丽         | 数字电路        |     85 |
| 105        | 3-105     | 王芳         | 计算机导论      |     88 |
| 105        | 3-245     | 王芳         | 操作系统        |     75 |
| 105        | 6-166     | 王芳         | 数字电路        |     79 |
| 109        | 3-105     | 赵铁柱       | 计算机导论      |     76 |
| 109        | 3-245     | 赵铁柱       | 操作系统        |     68 |
| 109        | 6-166     | 赵铁柱       | 数字电路        |     81 |
+------------+-----------+--------------+-----------------+--------+
9 rows in set (0.00 sec)

-- 添加查询字段
mysql> select student.student_id,course.course_id,score.student_id,score.course_id,student_name,course_name,degree from student,course,score where student.student_id=score.student_id and course.course_id=score.course_id;
+------------+-----------+------------+-----------+--------------+-----------------+--------+
| student_id | course_id | student_id | course_id | student_name | course_name     | degree |
+------------+-----------+------------+-----------+--------------+-----------------+--------+
| 103        | 3-105     | 103        | 3-105     | 王丽         | 计算机导论      |     92 |
| 103        | 3-245     | 103        | 3-245     | 王丽         | 操作系统        |     86 |
| 103        | 6-166     | 103        | 6-166     | 王丽         | 数字电路        |     85 |
| 105        | 3-105     | 105        | 3-105     | 王芳         | 计算机导论      |     88 |
| 105        | 3-245     | 105        | 3-245     | 王芳         | 操作系统        |     75 |
| 105        | 6-166     | 105        | 6-166     | 王芳         | 数字电路        |     79 |
| 109        | 3-105     | 109        | 3-105     | 赵铁柱       | 计算机导论      |     76 |
| 109        | 3-245     | 109        | 3-245     | 赵铁柱       | 操作系统        |     68 |
| 109        | 6-166     | 109        | 6-166     | 赵铁柱       | 数字电路        |     81 |
+------------+-----------+------------+-----------+--------------+-----------------+--------+
9 rows in set (0.00 sec)
```
25. 查询'95031'班学生每门课的平均分
```
# 第一种方法
mysql> select course_id,avg(degree) from student,score where student.student_id=score.student_id and class='95031' group by course_id;
+-----------+-------------+
| course_id | avg(degree) |
+-----------+-------------+
| 3-105     |     82.0000 |
| 3-245     |     71.5000 |
| 6-166     |     80.0000 |
+-----------+-------------+
3 rows in set (0.01 sec)


mysql> select course_id,class,avg(degree) from student,score where student.student_id=score.student_id and class='95031' group by course_id;
+-----------+-------+-------------+
| course_id | class | avg(degree) |
+-----------+-------+-------------+
| 3-105     | 95031 |     82.0000 |
| 3-245     | 95031 |     71.5000 |
| 6-166     | 95031 |     80.0000 |
+-----------+-------+-------------+
3 rows in set (0.01 sec)


# 第二种方法：
-- 第一步
mysql> select * from student where class='95031';
+------------+--------------+-------------+---------------------+-------+
| student_id | student_name | student_sex | student_birthday    | class |
+------------+--------------+-------------+---------------------+-------+
| 102        | 匡明         | 男          | 1975-10-02 00:00:00 | 95031 |
| 105        | 王芳         | 女          | 1975-02-10 00:00:00 | 95031 |
| 106        | 陆军         | 男          | 1974-06-03 00:00:00 | 95031 |
| 108        | 张全蛋       | 男          | 1975-02-10 00:00:00 | 95031 |
| 109        | 赵铁柱       | 男          | 1974-06-03 00:00:00 | 95031 |
+------------+--------------+-------------+---------------------+-------+
5 rows in set (0.00 sec)

-- 第二步
mysql> select * from score;
+------------+-----------+--------+
| student_id | course_id | degree |
+------------+-----------+--------+
| 103        | 3-105     |     92 |
| 103        | 3-245     |     86 |
| 103        | 6-166     |     85 |
| 105        | 3-105     |     88 |
| 105        | 3-245     |     75 |
| 105        | 6-166     |     79 |
| 109        | 3-105     |     76 |
| 109        | 3-245     |     68 |
| 109        | 6-166     |     81 |
+------------+-----------+--------+
9 rows in set (0.00 sec)

-- 第三步
mysql> select student_id from student where class='95031';
+------------+
| student_id |
+------------+
| 102        |
| 105        |
| 106        |
| 108        |
| 109        |
+------------+
5 rows in set (0.00 sec)

-- 第四步
mysql> select * from score where student_id in (select student_id from student where class='95031');
+------------+-----------+--------+
| student_id | course_id | degree |
+------------+-----------+--------+
| 105        | 3-105     |     88 |
| 105        | 3-245     |     75 |
| 105        | 6-166     |     79 |
| 109        | 3-105     |     76 |
| 109        | 3-245     |     68 |
| 109        | 6-166     |     81 |
+------------+-----------+--------+
6 rows in set (0.00 sec)

-- 最终答案
mysql> select course_id,avg(degree) from score where student_id in (select student_id from student where class='95031') group by course_id;
+-----------+-------------+
| course_id | avg(degree) |
+-----------+-------------+
| 3-105     |     82.0000 |
| 3-245     |     71.5000 |
| 6-166     |     80.0000 |
+-----------+-------------+
3 rows in set (0.00 sec)
```
26. 查询选修“3-105”课程的成绩高于“109”号同学“3-105”成绩的所有同学的记录
```
-- 第一步
mysql> select degree from score where student_id='109' and course_id='3-105';
+--------+
| degree |
+--------+
|     76 |
+--------+
1 row in set (0.00 sec)

-- 第二步
mysql> select * from score where degree >(select degree from score where student_id='109' and course_id='3-105');
+------------+-----------+--------+
| student_id | course_id | degree |
+------------+-----------+--------+
| 103        | 3-105     |     92 |
| 103        | 3-245     |     86 |
| 103        | 6-166     |     85 |
| 105        | 3-105     |     88 |
| 105        | 6-166     |     79 |
| 109        | 6-166     |     81 |
+------------+-----------+--------+
6 rows in set (0.01 sec)

-- 最终答案
mysql> select * from score where course_id='3-105' and degree >(select degree from score where student_id='109' and course_id='3-105');
+------------+-----------+--------+
| student_id | course_id | degree |
+------------+-----------+--------+
| 103        | 3-105     |     92 |
| 105        | 3-105     |     88 |
+------------+-----------+--------+
2 rows in set (0.00 sec)
```
27. 查询成绩高于学号为“109”、课程号为“3-105”的成绩的所有记录
```
mysql> select * from score where degree >(select degree from score where student_id='109' and course_id='3-105');
+------------+-----------+--------+
| student_id | course_id | degree |
+------------+-----------+--------+
| 103        | 3-105     |     92 |
| 103        | 3-245     |     86 |
| 103        | 6-166     |     85 |
| 105        | 3-105     |     88 |
| 105        | 6-166     |     79 |
| 109        | 6-166     |     81 |
+------------+-----------+--------+
6 rows in set (0.00 sec)
```
28. 查询和学号为108、101的同学同年出生的所有学生的student_id、student_name和student_birthday列
```
-- 第一步
mysql> select * from student where student_id in (108,101);
+------------+--------------+-------------+---------------------+-------+
| student_id | student_name | student_sex | student_birthday    | class |
+------------+--------------+-------------+---------------------+-------+
| 101        | 管华         | 男          | 1977-09-01 00:00:00 | 95033 |
| 108        | 张全蛋       | 男          | 1975-02-10 00:00:00 | 95031 |
+------------+--------------+-------------+---------------------+-------+
2 rows in set (0.00 sec)

-- 第二步
mysql> select year(student_birthday) from student where student_id in (108,101);
+------------------------+
| year(student_birthday) |
+------------------------+
|                   1977 |
|                   1975 |
+------------------------+
2 rows in set (0.00 sec)

-- 最后答案
mysql> select * from student where year(student_birthday) in (select year(student_birthday) from student where student_id in (108,101));
+------------+--------------+-------------+---------------------+-------+
| student_id | student_name | student_sex | student_birthday    | class |
+------------+--------------+-------------+---------------------+-------+
| 101        | 管华         | 男          | 1977-09-01 00:00:00 | 95033 |
| 102        | 匡明         | 男          | 1975-10-02 00:00:00 | 95031 |
| 105        | 王芳         | 女          | 1975-02-10 00:00:00 | 95031 |
| 108        | 张全蛋       | 男          | 1975-02-10 00:00:00 | 95031 |
+------------+--------------+-------------+---------------------+-------+
4 rows in set (0.00 sec)
```
29. 查询张旭教师任课学生的成绩
```
-- 多层嵌套子查询
mysql> select student_id,course_id,degree from score where course_id in(select course_id from course where teacher_id in(select teacher_id from teacher where teacher_name='张旭'));
+------------+-----------+--------+
| student_id | course_id | degree |
+------------+-----------+--------+
| 103        | 6-166     |     85 |
| 105        | 6-166     |     79 |
| 109        | 6-166     |     81 |
+------------+-----------+--------+
3 rows in set (0.00 sec)
```
30. 查询选修某课程的同学人数多于5人的教师姓名
```
-- 再度插入数据
insert into score values('101','3-105','90');
insert into score values('102','3-105','91');
insert into score values('104','3-105','89');

-- 第一步
mysql> select course_id,count(student_id) from score group by course_id;
+-----------+-------------------+
| course_id | count(student_id) |
+-----------+-------------------+
| 3-105     |                 6 |
| 3-245     |                 3 |
| 6-166     |                 3 |
+-----------+-------------------+
3 rows in set (0.00 sec)

-- 第二步
mysql> select course_id from score group by course_id having count(student_id)>5;
+-----------+
| course_id |
+-----------+
| 3-105     |
+-----------+
1 row in set (0.00 sec)

-- 第三步
mysql> select teacher_id from course where course_id in(select course_id from score group by course_id having count(student_id)>5);
+------------+
| teacher_id |
+------------+
| 825        |
+------------+
1 row in set (0.00 sec)

-- 最终答案
mysql> select teacher_name from teacher where teacher_id in(select teacher_id from course where course_id in(select course_id from score group by course_id having count(student_id)>5));
+--------------+
| teacher_name |
+--------------+
| 王萍         |
+--------------+
1 row in set (0.01 sec)
```
31. 查询95033班和95031班全体学生的记录
```
-- 再度插入数据
insert into student values('110','张飞','男','1974-06-03','95038');

-- in
mysql> select * from student where class in (95033,95031);
+------------+--------------+-------------+---------------------+-------+
| student_id | student_name | student_sex | student_birthday    | class |
+------------+--------------+-------------+---------------------+-------+
| 101        | 管华         | 男          | 1977-09-01 00:00:00 | 95033 |
| 102        | 匡明         | 男          | 1975-10-02 00:00:00 | 95031 |
| 103        | 王丽         | 女          | 1976-01-23 00:00:00 | 95033 |
| 104        | 李军         | 男          | 1976-02-20 00:00:00 | 95033 |
| 105        | 王芳         | 女          | 1975-02-10 00:00:00 | 95031 |
| 106        | 陆军         | 男          | 1974-06-03 00:00:00 | 95031 |
| 107        | 王尼玛       | 男          | 1976-02-20 00:00:00 | 95033 |
| 108        | 张全蛋       | 男          | 1975-02-10 00:00:00 | 95031 |
| 109        | 赵铁柱       | 男          | 1974-06-03 00:00:00 | 95031 |
+------------+--------------+-------------+---------------------+-------+
9 rows in set (0.00 sec)
```
32. 查询存在有85分以上成绩的课程course_id
```
mysql> select course_id,degree from score where degree > 85;
+-----------+--------+
| course_id | degree |
+-----------+--------+
| 3-105     |     90 |
| 3-105     |     91 |
| 3-105     |     92 |
| 3-245     |     86 |
| 3-105     |     89 |
| 3-105     |     88 |
+-----------+--------+
6 rows in set (0.00 sec)
```
33. 查询出“计算机系”教师所教课程的成绩表
```
-- 第一步
mysql> select teacher_id from teacher where teacher_department='计算机系';
+------------+
| teacher_id |
+------------+
| 804        |
| 825        |
+------------+
2 rows in set (0.00 sec)

-- 第二步
mysql> select course_id from course where teacher_id in (select teacher_id from teacher where teacher_department='计算机系');
+-----------+
| course_id |
+-----------+
| 3-245     |
| 3-105     |
+-----------+
2 rows in set (0.00 sec)

-- 最终答案
mysql> select * from score where course_id in (select course_id from course where teacher_id in (select teacher_id from teacher where teacher_department='计算机系'));
+------------+-----------+--------+
| student_id | course_id | degree |
+------------+-----------+--------+
| 103        | 3-245     |     86 |
| 105        | 3-245     |     75 |
| 109        | 3-245     |     68 |
| 101        | 3-105     |     90 |
| 102        | 3-105     |     91 |
| 103        | 3-105     |     92 |
| 104        | 3-105     |     89 |
| 105        | 3-105     |     88 |
| 109        | 3-105     |     76 |
+------------+-----------+--------+
9 rows in set (0.00 sec)
```
34. 查询”计算机系“与”电子工程系“不同职称的教师的teacher_name和teacher_profession
```
# union求并集
-- 第一步
mysql> select teacher_profession from teacher where teacher_department='计算机系';
+--------------------+
| teacher_profession |
+--------------------+
| 副教授             |
| 助教               |
+--------------------+
2 rows in set (0.00 sec)

-- 第二步
mysql> select teacher_profession from teacher where teacher_department='电子工程系';
+--------------------+
| teacher_profession |
+--------------------+
| 助教               |
| 讲师               |
+--------------------+
2 rows in set (0.00 sec)

-- 第三步
mysql> select * from teacher where teacher_department='电子工程系' and teacher_profession not in (select teacher_profession from teacher where teacher_department='计算机系');
+------------+--------------+-------------+---------------------+--------------------+--------------------+
| teacher_id | teacher_name | teacher_sex | teacher_birthday    | teacher_profession | teacher_department |
+------------+--------------+-------------+---------------------+--------------------+--------------------+
| 856        | 张旭         | 男          | 1969-03-12 00:00:00 | 讲师               | 电子工程系         |
+------------+--------------+-------------+---------------------+--------------------+--------------------+
1 row in set (0.00 sec)

-- 第四步
mysql> select * from teacher where teacher_department='计算机系' and teacher_profession not in (select teacher_profession from teacher where teacher_department='电子工程系');
+------------+--------------+-------------+---------------------+--------------------+--------------------+
| teacher_id | teacher_name | teacher_sex | teacher_birthday    | teacher_profession | teacher_department |
+------------+--------------+-------------+---------------------+--------------------+--------------------+
| 804        | 李城         | 男          | 1958-12-02 00:00:00 | 副教授             | 计算机系           |
+------------+--------------+-------------+---------------------+--------------------+--------------------+
1 row in set (0.00 sec)

-- 最终结果
mysql> select teacher_name,teacher_profession from teacher where teacher_department='计算机系' and teacher_profession not in (select teacher_profession from teacher where teacher_department='电子工程系') union select teacher_name,teacher_profession from teacher where teacher_department='电子工程系' and teacher_profession not in (select teacher_profession from teacher where teacher_department='计算机系');
+--------------+--------------------+
| teacher_name | teacher_profession |
+--------------+--------------------+
| 李城         | 副教授             |
| 张旭         | 讲师               |
+--------------+--------------------+
2 rows in set (0.01 sec)
```
35. 查询选修编号为”3-105“课程且成绩至少高于选修编号为”3-245“的同学的course_id、stdeunt_id和degree，并按degree从高到低次序排序
```
# 自己的写法
mysql> select * from score where course_id='3-105' and degree >(select min(degree) from score where course_id='3-245') order by degree desc;
+------------+-----------+--------+
| student_id | course_id | degree |
+------------+-----------+--------+
| 103        | 3-105     |     92 |
| 102        | 3-105     |     91 |
| 101        | 3-105     |     90 |
| 104        | 3-105     |     89 |
| 105        | 3-105     |     88 |
| 109        | 3-105     |     76 |
+------------+-----------+--------+
6 rows in set (0.00 sec)

# 老师写法
-- 第一步
mysql> select * from score where course_id='3-245';
+------------+-----------+--------+
| student_id | course_id | degree |
+------------+-----------+--------+
| 103        | 3-245     |     86 |
| 105        | 3-245     |     75 |
| 109        | 3-245     |     68 |
+------------+-----------+--------+
3 rows in set (0.01 sec)

-- 第二步
mysql> select * from score where course_id='3-105';
+------------+-----------+--------+
| student_id | course_id | degree |
+------------+-----------+--------+
| 101        | 3-105     |     90 |
| 102        | 3-105     |     91 |
| 103        | 3-105     |     92 |
| 104        | 3-105     |     89 |
| 105        | 3-105     |     88 |
| 109        | 3-105     |     76 |
+------------+-----------+--------+
6 rows in set (0.01 sec)

-- 最后答案
-- 至少 其中一个，any
mysql> select * from score where course_id='3-105' and degree >any(select degree from score where course_id='3-245') order by degree desc;
+------------+-----------+--------+
| student_id | course_id | degree |
+------------+-----------+--------+
| 103        | 3-105     |     92 |
| 102        | 3-105     |     91 |
| 101        | 3-105     |     90 |
| 104        | 3-105     |     89 |
| 105        | 3-105     |     88 |
| 109        | 3-105     |     76 |
+------------+-----------+--------+
6 rows in set (0.00 sec)
```
36. 查询选修编号为”3-105“且成绩高于选修编号为”3-245“课程的同学的course_id、student_id和degree
```
# 自己写法
mysql> select * from score where course_id='3-105' and degree >(select max(degree) from score where course_id='3-245');
+------------+-----------+--------+
| student_id | course_id | degree |
+------------+-----------+--------+
| 101        | 3-105     |     90 |
| 102        | 3-105     |     91 |
| 103        | 3-105     |     92 |
| 104        | 3-105     |     89 |
| 105        | 3-105     |     88 |
+------------+-----------+--------+
5 rows in set (0.01 sec)

# 老师写法

-- 且 all(表示所有的关系)
mysql> select * from score where course_id='3-105' and degree >all(select degree from score where course_id='3-245');
+------------+-----------+--------+
| student_id | course_id | degree |
+------------+-----------+--------+
| 101        | 3-105     |     90 |
| 102        | 3-105     |     91 |
| 103        | 3-105     |     92 |
| 104        | 3-105     |     89 |
| 105        | 3-105     |     88 |
+------------+-----------+--------+
5 rows in set (0.00 sec)
```
37. 查询所有教师和同学的name、sex和birthday
```
# 自己写法
mysql> select teacher_name,teacher_birthday,teacher_sex,student_name,student_sex,student_birthday from score,student,course,teacher where teacher.teacher_id=course.teacher_id and course.course_id=score.course_id and score.student_id=student.student_id;
+--------------+---------------------+-------------+--------------+-------------+---------------------+
| teacher_name | teacher_birthday    | teacher_sex | student_name | student_sex | student_birthday    |
+--------------+---------------------+-------------+--------------+-------------+---------------------+
| 李城         | 1958-12-02 00:00:00 | 男          | 王丽         | 女          | 1976-01-23 00:00:00 |
| 李城         | 1958-12-02 00:00:00 | 男          | 王芳         | 女          | 1975-02-10 00:00:00 |
| 李城         | 1958-12-02 00:00:00 | 男          | 赵铁柱       | 男          | 1974-06-03 00:00:00 |
| 王萍         | 1972-05-05 00:00:00 | 女          | 管华         | 男          | 1977-09-01 00:00:00 |
| 王萍         | 1972-05-05 00:00:00 | 女          | 匡明         | 男          | 1975-10-02 00:00:00 |
| 王萍         | 1972-05-05 00:00:00 | 女          | 王丽         | 女          | 1976-01-23 00:00:00 |
| 王萍         | 1972-05-05 00:00:00 | 女          | 李军         | 男          | 1976-02-20 00:00:00 |
| 王萍         | 1972-05-05 00:00:00 | 女          | 王芳         | 女          | 1975-02-10 00:00:00 |
| 王萍         | 1972-05-05 00:00:00 | 女          | 赵铁柱       | 男          | 1974-06-03 00:00:00 |
| 张旭         | 1969-03-12 00:00:00 | 男          | 王丽         | 女          | 1976-01-23 00:00:00 |
| 张旭         | 1969-03-12 00:00:00 | 男          | 王芳         | 女          | 1975-02-10 00:00:00 |
| 张旭         | 1969-03-12 00:00:00 | 男          | 赵铁柱       | 男          | 1974-06-03 00:00:00 |
+--------------+---------------------+-------------+--------------+-------------+---------------------+
12 rows in set (0.00 sec)

# 老师做法

-- 别名 as
-- 第一步
mysql> select teacher_name,teacher_sex,teacher_birthday from teacher;
+--------------+-------------+---------------------+
| teacher_name | teacher_sex | teacher_birthday    |
+--------------+-------------+---------------------+
| 李城         | 男          | 1958-12-02 00:00:00 |
| 王萍         | 女          | 1972-05-05 00:00:00 |
| 刘冰         | 女          | 1977-08-14 00:00:00 |
| 张旭         | 男          | 1969-03-12 00:00:00 |
+--------------+-------------+---------------------+
4 rows in set (0.00 sec)

-- 第二步
mysql> select student_name, student_sex,student_birthday from student;
+--------------+-------------+---------------------+
| student_name | student_sex | student_birthday    |
+--------------+-------------+---------------------+
| 管华         | 男          | 1977-09-01 00:00:00 |
| 匡明         | 男          | 1975-10-02 00:00:00 |
| 王丽         | 女          | 1976-01-23 00:00:00 |
| 李军         | 男          | 1976-02-20 00:00:00 |
| 王芳         | 女          | 1975-02-10 00:00:00 |
| 陆军         | 男          | 1974-06-03 00:00:00 |
| 王尼玛       | 男          | 1976-02-20 00:00:00 |
| 张全蛋       | 男          | 1975-02-10 00:00:00 |
| 赵铁柱       | 男          | 1974-06-03 00:00:00 |
| 张飞         | 男          | 1974-06-03 00:00:00 |
+--------------+-------------+---------------------+
10 rows in set (0.00 sec)

-- 最终答案
mysql> select teacher_name as name,teacher_sex as sex,teacher_birthday as birthday from teacher union select student_name, student_sex,student_birthday from student;
+-----------+------+---------------------+
| name      | sex  | birthday            |
+-----------+------+---------------------+
| 李城      | 男   | 1958-12-02 00:00:00 |
| 王萍      | 女   | 1972-05-05 00:00:00 |
| 刘冰      | 女   | 1977-08-14 00:00:00 |
| 张旭      | 男   | 1969-03-12 00:00:00 |
| 管华      | 男   | 1977-09-01 00:00:00 |
| 匡明      | 男   | 1975-10-02 00:00:00 |
| 王丽      | 女   | 1976-01-23 00:00:00 |
| 李军      | 男   | 1976-02-20 00:00:00 |
| 王芳      | 女   | 1975-02-10 00:00:00 |
| 陆军      | 男   | 1974-06-03 00:00:00 |
| 王尼玛    | 男   | 1976-02-20 00:00:00 |
| 张全蛋    | 男   | 1975-02-10 00:00:00 |
| 赵铁柱    | 男   | 1974-06-03 00:00:00 |
| 张飞      | 男   | 1974-06-03 00:00:00 |
+-----------+------+---------------------+
14 rows in set (0.00 sec)
```
38. 查询所有“女教师”和“女”同学的name、sex和birthday
```
-- 第一步
mysql> select * from teacher where teacher_sex='女';
+------------+--------------+-------------+---------------------+--------------------+--------------------+
| teacher_id | teacher_name | teacher_sex | teacher_birthday    | teacher_profession | teacher_department |
+------------+--------------+-------------+---------------------+--------------------+--------------------+
| 825        | 王萍         | 女          | 1972-05-05 00:00:00 | 助教               | 计算机系           |
| 831        | 刘冰         | 女          | 1977-08-14 00:00:00 | 助教               | 电子工程系         |
+------------+--------------+-------------+---------------------+--------------------+--------------------+
2 rows in set (0.00 sec)

-- 第二步
mysql> select * from student where student_sex='女';
+------------+--------------+-------------+---------------------+-------+
| student_id | student_name | student_sex | student_birthday    | class |
+------------+--------------+-------------+---------------------+-------+
| 103        | 王丽         | 女          | 1976-01-23 00:00:00 | 95033 |
| 105        | 王芳         | 女          | 1975-02-10 00:00:00 | 95031 |
+------------+--------------+-------------+---------------------+-------+
2 rows in set (0.00 sec)

-- 最终答案
mysql> select student_name as name,student_sex as sex,student_birthday as birthday from student where student_sex='女' union select teacher_name,teacher_sex,teacher_birthday from teacher where teacher_sex='女';
+--------+------+---------------------+
| name   | sex  | birthday            |
+--------+------+---------------------+
| 王丽   | 女   | 1976-01-23 00:00:00 |
| 王芳   | 女   | 1975-02-10 00:00:00 |
| 王萍   | 女   | 1972-05-05 00:00:00 |
| 刘冰   | 女   | 1977-08-14 00:00:00 |
+--------+------+---------------------+
4 rows in set (0.00 sec)
```
39. 查询成绩比该课程平均成绩低的同学的成绩表
```
-- 第一步
-- 数据修改
insert into score values('109','6-166','68');

mysql> select course_id,avg(degree) from score group by course_id;
+-----------+-------------+
| course_id | avg(degree) |
+-----------+-------------+
| 3-105     |     87.6667 |
| 3-245     |     76.3333 |
| 6-166     |     77.3333 |
+-----------+-------------+
3 rows in set (0.00 sec)

-- 第二步
mysql> select * from score;  
-- 表a        
+------------+-----------+--------+           
| student_id | course_id | degree |           
+------------+-----------+--------+
| 101        | 3-105     |     90 |
| 102        | 3-105     |     91 |
| 103        | 3-105     |     92 |
| 103        | 3-245     |     86 |
| 103        | 6-166     |     85 |
| 104        | 3-105     |     89 |
| 105        | 3-105     |     88 |
| 105        | 3-245     |     75 |
| 105        | 6-166     |     79 |
| 109        | 3-105     |     76 |
| 109        | 3-245     |     68 |         
| 109        | 6-166     |     68 |
+------------+-----------+--------+
12 rows in set (0.00 sec)

-- 表b
+------------+-----------+--------+
| student_id | course_id | degree |
+------------+-----------+--------+
| 101        | 3-105     |     90 |
| 102        | 3-105     |     91 |
| 103        | 3-105     |     92 |
| 103        | 3-245     |     86 |
| 103        | 6-166     |     85 |
| 104        | 3-105     |     89 |
| 105        | 3-105     |     88 |
| 105        | 3-245     |     75 |
| 105        | 6-166     |     79 |
| 109        | 3-105     |     76 |
| 109        | 3-245     |     68 |
| 109        | 6-166     |     68 |
+------------+-----------+--------+
12 rows in set (0.00 sec)

-- 最终答案
mysql> select * from score a where degree < (select avg(degree) from score b where a.course_id= b.course_id);
+------------+-----------+--------+
| student_id | course_id | degree |
+------------+-----------+--------+
| 105        | 3-245     |     75 |
| 109        | 3-105     |     76 |
| 109        | 3-245     |     68 |
| 109        | 6-166     |     68 |
+------------+-----------+--------+
4 rows in set (0.00 sec)
```
40. 查询所有任课教师的teacher_name和teacher_department
```
-- 第一步
mysql> select * from course;
+-----------+-----------------+------------+
| course_id | course_name     | teacher_id |
+-----------+-----------------+------------+
| 3-105     | 计算机导论      | 825        |
| 3-245     | 操作系统        | 804        |
| 6-166     | 数字电路        | 856        |
| 9-888     | 高等数学        | 831        |
+-----------+-----------------+------------+
4 rows in set (0.00 sec)

-- 最终答案
mysql> select teacher_name,teacher_department from teacher where teacher_id in (select teacher_id from course);
+--------------+--------------------+
| teacher_name | teacher_department |
+--------------+--------------------+
| 李城         | 计算机系           |
| 王萍         | 计算机系           |
| 刘冰         | 电子工程系         |
| 张旭         | 电子工程系         |
+--------------+--------------------+
4 rows in set (0.01 sec)
```
41. 查询至少有两名学生的班号
```
-- 第一步
mysql> select * from student;
+------------+--------------+-------------+---------------------+-------+
| student_id | student_name | student_sex | student_birthday    | class |
+------------+--------------+-------------+---------------------+-------+
| 101        | 管华         | 男          | 1977-09-01 00:00:00 | 95033 |
| 102        | 匡明         | 男          | 1975-10-02 00:00:00 | 95031 |
| 103        | 王丽         | 女          | 1976-01-23 00:00:00 | 95033 |
| 104        | 李军         | 男          | 1976-02-20 00:00:00 | 95033 |
| 105        | 王芳         | 女          | 1975-02-10 00:00:00 | 95031 |
| 106        | 陆军         | 男          | 1974-06-03 00:00:00 | 95031 |
| 107        | 王尼玛       | 男          | 1976-02-20 00:00:00 | 95033 |
| 108        | 张全蛋       | 男          | 1975-02-10 00:00:00 | 95031 |
| 109        | 赵铁柱       | 男          | 1974-06-03 00:00:00 | 95031 |
| 110        | 张飞         | 男          | 1974-06-03 00:00:00 | 95038 |
+------------+--------------+-------------+---------------------+-------+
10 rows in set (0.00 sec)

-- 最终答案
mysql> select class from student where student_sex='男' group by class having count(*)>2;
+-------+
| class |
+-------+
| 95031 |
| 95033 |
+-------+
2 rows in set (0.00 sec)
```
42. 查询student表中不姓“王”的同学记录
```
mysql> select * from student where student_name not like '王%';
+------------+--------------+-------------+---------------------+-------+
| student_id | student_name | student_sex | student_birthday    | class |
+------------+--------------+-------------+---------------------+-------+
| 101        | 管华         | 男          | 1977-09-01 00:00:00 | 95033 |
| 102        | 匡明         | 男          | 1975-10-02 00:00:00 | 95031 |
| 104        | 李军         | 男          | 1976-02-20 00:00:00 | 95033 |
| 106        | 陆军         | 男          | 1974-06-03 00:00:00 | 95031 |
| 108        | 张全蛋       | 男          | 1975-02-10 00:00:00 | 95031 |
| 109        | 赵铁柱       | 男          | 1974-06-03 00:00:00 | 95031 |
| 110        | 张飞         | 男          | 1974-06-03 00:00:00 | 95038 |
+------------+--------------+-------------+---------------------+-------+
7 rows in set (0.00 sec)
```
43. 查询student表中的每个学生的姓名和年龄
```
# 年龄=当前年份-出生年份

-- 第一步
mysql> select year(now());
+-------------+
| year(now()) |
+-------------+
|        2019 |
+-------------+
1 row in set (0.00 sec)

-- 第二步
mysql> select year(student_birthday) from student;
+------------------------+
| year(student_birthday) |
+------------------------+
|                   1977 |
|                   1975 |
|                   1976 |
|                   1976 |
|                   1975 |
|                   1974 |
|                   1976 |
|                   1975 |
|                   1974 |
|                   1974 |
+------------------------+
10 rows in set (0.00 sec)


-- 最终答案
mysql> select student_name,year(now())-year(student_birthday) as age from student;
+--------------+------+
| student_name | age  |
+--------------+------+
| 管华         |   42 |
| 匡明         |   44 |
| 王丽         |   43 |
| 李军         |   43 |
| 王芳         |   44 |
| 陆军         |   45 |
| 王尼玛       |   43 |
| 张全蛋       |   44 |
| 赵铁柱       |   45 |
| 张飞         |   45 |
+--------------+------+
10 rows in set (0.01 sec)
```
44. 查询student表中最大、最小的student_birthday日期值
```
-- max、min
mysql> select max(student_birthday),min(student_birthday) from student;
+-----------------------+-----------------------+
| max(student_birthday) | min(student_birthday) |
+-----------------------+-----------------------+
| 1977-09-01 00:00:00   | 1974-06-03 00:00:00   |
+-----------------------+-----------------------+
1 row in set (0.00 sec)
```
45. 以班号和年龄从大到小的顺序查询student表中的全部记录
```
mysql> select * from student order by class desc,student_birthday;
+------------+--------------+-------------+---------------------+-------+
| student_id | student_name | student_sex | student_birthday    | class |
+------------+--------------+-------------+---------------------+-------+
| 110        | 张飞         | 男          | 1974-06-03 00:00:00 | 95038 |
| 103        | 王丽         | 女          | 1976-01-23 00:00:00 | 95033 |
| 104        | 李军         | 男          | 1976-02-20 00:00:00 | 95033 |
| 107        | 王尼玛       | 男          | 1976-02-20 00:00:00 | 95033 |
| 101        | 管华         | 男          | 1977-09-01 00:00:00 | 95033 |
| 106        | 陆军         | 男          | 1974-06-03 00:00:00 | 95031 |
| 109        | 赵铁柱       | 男          | 1974-06-03 00:00:00 | 95031 |
| 105        | 王芳         | 女          | 1975-02-10 00:00:00 | 95031 |
| 108        | 张全蛋       | 男          | 1975-02-10 00:00:00 | 95031 |
| 102        | 匡明         | 男          | 1975-10-02 00:00:00 | 95031 |
+------------+--------------+-------------+---------------------+-------+
10 rows in set (0.00 sec)
```
46. 查询“男”教师及其所上的课程
```
-- 第一步
mysql> select teacher_id from teacher where teacher_sex='男';
+------------+
| teacher_id |
+------------+
| 804        |
| 856        |
+------------+
2 rows in set (0.00 sec)

-- 最终答案
mysql> select * from course where teacher_id in (select teacher_id from teacher where teacher_sex='男');
+-----------+--------------+------------+
| course_id | course_name  | teacher_id |
+-----------+--------------+------------+
| 3-245     | 操作系统     | 804        |
| 6-166     | 数字电路     | 856        |
+-----------+--------------+------------+
2 rows in set (0.00 sec)
```
47. 查询最高分同学的student_id、course_id和degree列
```
mysql> select * from score where degree in (select max(degree) from score);
+------------+-----------+--------+
| student_id | course_id | degree |
+------------+-----------+--------+
| 103        | 3-105     |     92 |
+------------+-----------+--------+
1 row in set (0.00 sec)
```
48. 查询和”李军“同性别的所有同学的student_name
```
mysql> select student_name from student where student_sex in (select student_sex from student where student_name='李军');
+--------------+
| student_name |
+--------------+
| 管华         |
| 匡明         |
| 李军         |
| 陆军         |
| 王尼玛       |
| 张全蛋       |
| 赵铁柱       |
| 张飞         |
+--------------+
8 rows in set (0.00 sec)
```
49. 查询和”李军“同性别并同班的同学的student_name
```
mysql> select student_name from student where student_sex in (select student_sex from student where student_name='李军') and class in (select class from student where student_name='李军');
+--------------+
| student_name |
+--------------+
| 管华         |
| 李军         |
| 王尼玛       |
+--------------+
3 rows in set (0.00 sec)
```
50. 查询所有选修”计算机导论“课程的”男“同学的成绩表
```
-- 第一步
mysql> select student_id from student where student_sex='男';
+------------+
| student_id |
+------------+
| 101        |
| 102        |
| 104        |
| 106        |
| 107        |
| 108        |
| 109        |
| 110        |
+------------+
8 rows in set (0.00 sec)


-- 第二步
mysql> select course_id from course where course_name='计算机导论';
+-----------+
| course_id |
+-----------+
| 3-105     |
+-----------+
1 row in set (0.00 sec)


-- 最终答案
mysql> select * from score where course_id in (select course_id from course where course_name='计算机导论') and student_id in (select student_id from student where student_sex='男');
+------------+-----------+--------+
| student_id | course_id | degree |
+------------+-----------+--------+
| 101        | 3-105     |     90 |
| 102        | 3-105     |     91 |
| 104        | 3-105     |     89 |
| 109        | 3-105     |     76 |
+------------+-----------+--------+
4 rows in set (0.00 sec)
```
51. 假设使用如下命令建立了一个grade表，现查询所有同学的student_id、course_id和grade列
```
create table grade(
	low int(3),
	upp int(3),
	grade char(1)
);


insert into grade values(90,100,'A');
insert into grade values(80,89,'B');
insert into grade values(70,79,'C');
insert into grade values(60,69,'D');
insert into grade values(0,59,'E');


mysql> select student_id,course_id,grade from score,grade where degree between low and upp;
+------------+-----------+-------+
| student_id | course_id | grade |
+------------+-----------+-------+
| 101        | 3-105     | A     |
| 102        | 3-105     | A     |
| 103        | 3-105     | A     |
| 103        | 3-245     | B     |
| 103        | 6-166     | B     |
| 104        | 3-105     | B     |
| 105        | 3-105     | B     |
| 105        | 3-245     | C     |
| 105        | 6-166     | C     |
| 109        | 3-105     | C     |
| 109        | 3-245     | D     |
| 109        | 6-166     | D     |
+------------+-----------+-------+
12 rows in set (0.00 sec)
```
