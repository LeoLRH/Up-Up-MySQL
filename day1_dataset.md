# Day1-数据库创建与表创建及一些查询

## 0.关于我

- 个人网站

  https://light-city.club

- 个人微信公众号

![wechat](./img/wechat.jpg)

## 1.创建数据库

```mysql
mysql> create database pratice;
Query OK, 1 row affected (0.04 sec)

mysql> use pratice;
Database changed
```

## 2.创建数据库表

### 2.1 表结构

> 学生表

Student(SId,Sname,Sage,Ssex)
SId 学生编号,Sname 学生姓名,Sage 出生年月,Ssex 学生性别

> 课程表

Course(CId,Cname,TId)
CId 课程编号,Cname 课程名称,TId 教师编号

> 教师表

Teacher(TId,Tname)
TId 教师编号,Tname 教师姓名

> 成绩表

SC(SId,CId,score)
SId 学生编号,CId 课程编号,score 分数

### 2.2 表创建

```mysql
mysql> create table Student(SId varchar(10) PRIMARY KEY NOT NULL,Sname varchar(10),Sage datetime,Ssex enum('男','女','保密') default '保密');
Query OK, 0 rows affected (0.36 sec)

    mysql> create table Course(CId varchar(10) PRIMARY KEY NOT NULL,Cname varchar(10),TId varchar(10));
Query OK, 0 rows affected (0.37 sec)

mysql> create table Teacher(TId varchar(10)PRIMARY KEY NOT NULL,Tname varchar(10));
Query OK, 0 rows affected (0.37 sec)

mysql> create table SC(SId varchar(10) NOT NULL,CId varchar(10) NOT NULL,score decimal(5,1));
Query OK, 0 rows affected (0.33 sec)
```

### 2.3 数据插入

#### 2.3.1 Student表

导入数据：

```mysql
load data local infile "/home/light/mysql/practice/student.txt" into table Student fields terminated by ',';
```

查询数据：

```mysql
mysql> select * from Student;
+-----+--------+---------------------+------+
| SId | Sname  | Sage                | Ssex |
+-----+--------+---------------------+------+
| 01  | 赵雷   | 1990-01-01 00:00:00 | 男   |
| 02  | 钱电   | 1990-12-21 00:00:00 | 男   |
| 03  | 孙风   | 1990-12-20 00:00:00 | 男   |
| 04  | 李云   | 1990-12-06 00:00:00 | 男   |
| 05  | 周梅   | 1991-12-01 00:00:00 | 女   |
| 06  | 吴兰   | 1992-01-01 00:00:00 | 女   |
| 07  | 郑竹   | 1989-01-01 00:00:00 | 女   |
| 09  | 张三   | 2017-12-20 00:00:00 | 女   |
| 10  | 李四   | 2017-12-25 00:00:00 | 女   |
| 11  | 李四   | 2012-06-06 00:00:00 | 女   |
| 12  | 赵六   | 2013-06-13 00:00:00 | 女   |
| 13  | 孙七   | 2014-06-01 00:00:00 | 女   |
+-----+--------+---------------------+------+
12 rows in set (0.00 sec)
```

#### 2.3.2 Course表

导入表：

```mysql
mysql> load data local infile "/home/light/mysql/practice/course.txt" into table Course fields terminated by ',';
Query OK, 3 rows affected (0.09 sec)
Records: 3  Deleted: 0  Skipped: 0  Warnings: 0
```

查询表：

```mysql
mysql> select * from Course;                                                 
| CId | Cname  | TId  |
+-----+--------+------+
| 01  | 语文   | 02   |
| 02  | 数学   | 01   |
| 03  | 英语   | 03   |
+-----+--------+------+
3 rows in set (0.00 sec)
```

#### 2.3.3 Teacher表

导入表：

```mysql
mysql> load data local infile "/home/light/mysql/practice/teacher.txt" into table Teacher fields terminated by ',';
Query OK, 3 rows affected (0.09 sec)
Records: 3  Deleted: 0  Skipped: 0  Warnings: 0
```

查询表：

```mysql
mysql> select * from Course;                                                 
| CId | Cname  | TId  |
+-----+--------+------+
| 01  | 语文   | 02   |
| 02  | 数学   | 01   |
| 03  | 英语   | 03   |
+-----+--------+------+
3 rows in set (0.00 sec)
```

#### 2.3.4 SC表

导入表：

```mysql
mysql> load data local infile "/home/light/mysql/practice/sc.txt" into table SC fields terminated by ',';
Query OK, 18 rows affected (0.14 sec)
Records: 18  Deleted: 0  Skipped: 0  Warnings: 0
```

查询表：

```mysql
mysql> select * from SC;
+-----+-----+-------+
| SId | CId | score |
+-----+-----+-------+
| 01  | 01  |  80.0 |
| 01  | 02  |  90.0 |
| 01  | 03  |  99.0 |
| 02  | 01  |  70.0 |
| 02  | 02  |  60.0 |
| 02  | 03  |  80.0 |
| 03  | 01  |  80.0 |
| 03  | 02  |  80.0 |
| 03  | 03  |  80.0 |
| 04  | 01  |  50.0 |
| 04  | 02  |  30.0 |
| 04  | 03  |  20.0 |
| 05  | 01  |  76.0 |
| 05  | 02  |  87.0 |
| 06  | 01  |  31.0 |
| 06  | 03  |  34.0 |
| 07  | 02  |  89.0 |
| 07  | 03  |  98.0 |
+-----+-----+-------+
18 rows in set (0.01 sec)
```

## 3.查询

> 查询出课程编号为'01'的学生信息与成绩

```mysql
mysql> select s.*,sc.score from Student s,Course c,SC sc where c.CId='01' and s.SId=sc.SId and c.CId=sc.CId;
+-----+--------+---------------------+------+-------+
| SId | Sname  | Sage                | Ssex | score |
+-----+--------+---------------------+------+-------+
| 01  | 赵雷   | 1990-01-01 00:00:00 | 男   |  80.0 |
| 02  | 钱电   | 1990-12-21 00:00:00 | 男   |  70.0 |
| 03  | 孙风   | 1990-12-20 00:00:00 | 男   |  80.0 |
| 04  | 李云   | 1990-12-06 00:00:00 | 男   |  50.0 |
| 05  | 周梅   | 1991-12-01 00:00:00 | 女   |  76.0 |
| 06  | 吴兰   | 1992-01-01 00:00:00 | 女   |  31.0 |
+-----+--------+---------------------+------+-------+
6 rows in set (0.02 sec)

mysql> select s.*,sc.score from SC sc join Student s on sc.SId=s.SId join Course c on sc.CId=c.CId where c.CId='01';
+-----+--------+---------------------+------+-------+
| SId | Sname  | Sage                | Ssex | score |
+-----+--------+---------------------+------+-------+
| 01  | 赵雷   | 1990-01-01 00:00:00 | 男   |  80.0 |
| 02  | 钱电   | 1990-12-21 00:00:00 | 男   |  70.0 |
| 03  | 孙风   | 1990-12-20 00:00:00 | 男   |  80.0 |
| 04  | 李云   | 1990-12-06 00:00:00 | 男   |  50.0 |
| 05  | 周梅   | 1991-12-01 00:00:00 | 女   |  76.0 |
| 06  | 吴兰   | 1992-01-01 00:00:00 | 女   |  31.0 |
+-----+--------+---------------------+------+-------+
6 rows in set (0.00 sec)
```

> 查询" 01 "课程比" 02 "课程成绩高的学生的信息及课程分数

```mysql
SELECT s1.*
FROM (
	SELECT s.*, sc.score
	FROM SC sc
		JOIN Student s ON sc.SId = s.SId
		JOIN Course c ON sc.CId = c.CId
	WHERE c.CId = '01'
) s1, (
		SELECT s.*, sc.score
		FROM SC sc
			JOIN Student s ON sc.SId = s.SId
			JOIN Course c ON sc.CId = c.CId
		WHERE c.CId = '02'
	) s2
WHERE s1.SId = s2.SId
	AND s1.score > s2.score;
```

由于要查询出学生的所有信息，所以上述的join要改为left/right join。

```mysql
SELECT *
FROM (
	SELECT s.*, sc.score
	FROM SC sc
		JOIN Student s ON sc.SId = s.SId
		JOIN Course c ON sc.CId = c.CId
	WHERE c.CId = '01'
) s1
	LEFT JOIN (
		SELECT s.*, sc.score
		FROM SC sc
			JOIN Student s ON sc.SId = s.SId
			JOIN Course c ON sc.CId = c.CId
		WHERE c.CId = '02'
	) s2
	ON s1.SId = s2.SId
WHERE s1.score > s2.score;
```

上述的`left`可以换成`right`。

```mysql
+-----+--------+---------------------+------+-------+------+--------+---------------------+------+-------+
| SId | Sname  | Sage                | Ssex | score | SId  | Sname  | Sage                | Ssex | score |
+-----+--------+---------------------+------+-------+------+--------+---------------------+------+-------+
| 02  | 钱电   | 1990-12-21 00:00:00 | 男   |  70.0 | 02   | 钱电   | 1990-12-21 00:00:00 | 男   |  60.0 |
| 04  | 李云   | 1990-12-06 00:00:00 | 男   |  50.0 | 04   | 李云   | 1990-12-06 00:00:00 | 男   |  30.0 |
+-----+--------+---------------------+------+-------+------+--------+---------------------+------+-------+
2 rows in set (0.00 sec)
```

> 查询同时存在" 01 "课程和" 02 "课程的情况

我们先通过两张子表得到课程01和课程02的成绩信息，然后笛卡儿积合并两张表，最后WHERE筛选

```mysql
mysql> select s1.*,s2.CId,s2.score as s2_score from (select * from SC sc where sc.CId='01') s1,(select * from SC sc where sc.CId='02') s2 where s1.SId=s2.SId;
+-----+-----+-------+-----+----------+
| SId | CId | score | CId | s2_score |
+-----+-----+-------+-----+----------+
| 01  | 01  |  80.0 | 02  |     90.0 |
| 02  | 01  |  70.0 | 02  |     60.0 |
| 03  | 01  |  80.0 | 02  |     80.0 |
| 04  | 01  |  50.0 | 02  |     30.0 |
| 05  | 01  |  76.0 | 02  |     87.0 |
+-----+-----+-------+-----+----------+
5 rows in set (0.00 sec)
```

分析：先分别查找出这个学生选修'01'或'02'的信息，然后通过学生的`SId`进行筛选，得到了这个学生同时存在" 01 "课程和" 02 "课程的情况。

如果对结果要求不高的，可以用in子查询。

```mysql
mysql> select * from SC sc where sc.CId='01' and sc.SId in (select sc.SId from SC sc where sc.CId='02');
+-----+-----+-------+
| SId | CId | score |
+-----+-----+-------+
| 01  | 01  |  80.0 |
| 02  | 01  |  70.0 |
| 03  | 01  |  80.0 |
| 04  | 01  |  50.0 |
| 05  | 01  |  76.0 |
+-----+-----+-------+
5 rows in set (0.00 sec)
```



> 查询存在" 01 "课程但可能不存在" 02 "课程的情况(不存在时显示为 null )

首先要明确查询的表是成绩表（`SC`）的信息，这里的主要难点是：不存在显示为NULL。

这一道就是明显需要使用join的情况了，02可能不存在，即为left join的右侧或right join 的左侧即可。

左连接：

```mysql
mysql> select * from (select * from SC sc where sc.CId='01')s1 left join (select * from SC sc where sc.CId='02')s2 on s1.SId=s2.SId;
+-----+-----+-------+------+------+-------+
| SId | CId | score | SId  | CId  | score |
+-----+-----+-------+------+------+-------+
| 01  | 01  |  80.0 | 01   | 02   |  90.0 |
| 02  | 01  |  70.0 | 02   | 02   |  60.0 |
| 03  | 01  |  80.0 | 03   | 02   |  80.0 |
| 04  | 01  |  50.0 | 04   | 02   |  30.0 |
| 05  | 01  |  76.0 | 05   | 02   |  87.0 |
| 06  | 01  |  31.0 | NULL | NULL |  NULL |
+-----+-----+-------+------+------+-------+
6 rows in set (0.00 sec)
```

右连接：

```mysql
mysql> select * from (select * from SC sc where sc.CId='02')s1 right join (select * from SC sc where sc.CId='01')s2 on s1.SId=s2.SId;
+------+------+-------+-----+-----+-------+
| SId  | CId  | score | SId | CId | score |
+------+------+-------+-----+-----+-------+
| 01   | 02   |  90.0 | 01  | 01  |  80.0 |
| 02   | 02   |  60.0 | 02  | 01  |  70.0 |
| 03   | 02   |  80.0 | 03  | 01  |  80.0 |
| 04   | 02   |  30.0 | 04  | 01  |  50.0 |
| 05   | 02   |  87.0 | 05  | 01  |  76.0 |
| NULL | NULL |  NULL | 06  | 01  |  31.0 |
+------+------+-------+-----+-----+-------+
6 rows in set (0.00 sec)
```

> 查询不存在" 01 "课程但存在" 02 "课程的情况

方法一：

in子查询过滤。

```mysql
mysql> select * from SC sc where sc.CId='02' and sc.SId not in (select sc.SId from SC sc where sc.CId='01');
+-----+-----+-------+
| SId | CId | score |
+-----+-----+-------+
| 07  | 02  |  89.0 |
+-----+-----+-------+
1 row in set (0.00 sec)
```

方法二：

首先查询存在" 02 "课程但可能不存在" 01 "课程的情况(不存在时显示为 null )

```
mysql> select * from (select * from SC sc where sc.CId='02')s1 left join (select * from SC sc where sc.CId='01')s2 on s1.SId=s2.SId;
+-----+-----+-------+------+------+-------+
| SId | CId | score | SId  | CId  | score |
+-----+-----+-------+------+------+-------+
| 01  | 02  |  90.0 | 01   | 01   |  80.0 |
| 02  | 02  |  60.0 | 02   | 01   |  70.0 |
| 03  | 02  |  80.0 | 03   | 01   |  80.0 |
| 04  | 02  |  30.0 | 04   | 01   |  50.0 |
| 05  | 02  |  87.0 | 05   | 01   |  76.0 |
| 07  | 02  |  89.0 | NULL | NULL |  NULL |
+-----+-----+-------+------+------+-------+
6 rows in set (0.00 sec)
```

然后使用WHERE过滤出不存在的01课程，也就是最后一条数据即可：

```mysql
mysql> select * from (select * from SC sc where sc.CId='02')s1 left join (select * from SC sc where sc.CId='01')s2 on s1.SId=s2.SId where s2.SId is NULL;
+-----+-----+-------+------+------+-------+
| SId | CId | score | SId  | CId  | score |
+-----+-----+-------+------+------+-------+
| 07  | 02  |  89.0 | NULL | NULL |  NULL |
+-----+-----+-------+------+------+-------+
1 row in set (0.00 sec)
```

> 查询平均成绩大于等于 60 分的同学的学生编号和学生姓名和平均成绩

```mysql
mysql> select s.SId,s.Sname,avg(sc.score) as AVG from Student s join SC sc on s.SId=sc.SId group by sc.SID having AVG>=60;
+-----+--------+----------+
| SId | Sname  | AVG      |
+-----+--------+----------+
| 01  | 赵雷   | 89.66667 |
| 02  | 钱电   | 70.00000 |
| 03  | 孙风   | 80.00000 |
| 05  | 周梅   | 81.50000 |
| 07  | 郑竹   | 93.50000 |
+-----+--------+----------+
5 rows in set (0.00 sec)
```

> 查询在 SC 表存在成绩的学生信息

方法一：使用`distinct`关键字

```mysql
mysql> select distinct s.* from Student s, SC sc where s.SId=sc.SId;
+-----+--------+---------------------+------+
| SId | Sname  | Sage                | Ssex |
+-----+--------+---------------------+------+
| 01  | 赵雷   | 1990-01-01 00:00:00 | 男   |
| 02  | 钱电   | 1990-12-21 00:00:00 | 男   |
| 03  | 孙风   | 1990-12-20 00:00:00 | 男   |
| 04  | 李云   | 1990-12-06 00:00:00 | 男   |
| 05  | 周梅   | 1991-12-01 00:00:00 | 女   |
| 06  | 吴兰   | 1992-01-01 00:00:00 | 女   |
| 07  | 郑竹   | 1989-01-01 00:00:00 | 女   |
+-----+--------+---------------------+------+
7 rows in set (0.00 sec)
```

方法二：使用`exists`关键字

```mysql
mysql> select * from Student s where exists(select sc.SId from SC sc where s.SId=sc.SId);
+-----+--------+---------------------+------+
| SId | Sname  | Sage                | Ssex |
+-----+--------+---------------------+------+
| 01  | 赵雷   | 1990-01-01 00:00:00 | 男   |
| 02  | 钱电   | 1990-12-21 00:00:00 | 男   |
| 03  | 孙风   | 1990-12-20 00:00:00 | 男   |
| 04  | 李云   | 1990-12-06 00:00:00 | 男   |
| 05  | 周梅   | 1991-12-01 00:00:00 | 女   |
| 06  | 吴兰   | 1992-01-01 00:00:00 | 女   |
| 07  | 郑竹   | 1989-01-01 00:00:00 | 女   |
+-----+--------+---------------------+------+
7 rows in set (0.00 sec)
```

