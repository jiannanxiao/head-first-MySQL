### 第二章 SQL基础

#### 2.1 SQL简介

SQL 是 Structure Query Language（结构化查询语言）的缩写，它是使用关系模型的数据库应用语言，由 IBM 在 20 世纪 70 年代开发出来，作为 IBM 关系数据库原型 System R 的原型关 系语言，实现了关系数据库中的信息检索。



#### 2.2 SQL使用入门



**2.2.1 SQL分类**

SQL语句主要分为三个类别：

- **DDL（Data Definition Languages）语句**：数据定义语言，这些语句定义了不同的数据段、 数据库、表、列、索引等数据库对象的定义。常用的语句关键字主要包括 create、drop、alter 等。 
- **DML（Data Manipulation Language）语句**：数据操纵语句，用于添加、删除、更新和查询数据库记录，并检查数据完整性，常用的语句关键字主要包括 insert、delete、udpate 和 select 等。 
- ** DCL（Data Control Language）语句**：数据控制语句，用于控制不同数据段直接的许可和 访问级别的语句。这些语句定义了数据库、表、字段、用户的访问权限和安全级别。主要的语句关键字包括 grant、revoke 等。



**2.2.2 DDL语句**

DDL 是数据定义语言的缩写，简单来说，就是对数据库内部的对象进行创建、删除、修改的 操作语言。**它和 DML 语言的最大区别是 DML 只是对表内部数据的操作，而不涉及到表的定义、结构的修改，更不会涉及到其他对象。**DDL 语句更多的被数据库管理员（DBA）所使用， 一般的开发人员很少使用。



1、创建数据库

启动 MySQL 服务之后，输入以下命令连接到 MySQL 服务器：

```mysql
$ -u root -p
Enter password:

/*在以上命令行中，mysql 代表客户端命令，-u 后面跟连接的数据库用户，-p 表示需要输入密码;
命令的结束符，用;或者\g 结束;
每个 SQL 语句以分号或者\g 结束，按回 车键执行。;*/
```



例如，创建数据库test1：

```mysql
mysql> CREATE database test1;
Query OK, 1 row affected (0.00 sec)
```



这个时候，如果需要知道系统中都存在哪些数据库，可以用以下命令来查看：

```mysql
mysql> show databases;
+--------------------+
| Database
+--------------------+
| information_schema |
| cluster
| mysql
| test
| test1
+--------------------+

5 rows in set (0.00 sec)

/*
可以发现，在上面的列表中除了刚刚创建的 test1 外，还有另外 4 个数据库，它们都是安装 MySQL 时系统自动创建的，其各自功能如下。  information_schema：主要存储了系统中的一些数据库对象信息。比如用户表信息、列信息、权限信息、字符集信息、分区信息等。 
cluster：存储了系统的集群信息。 
mysql：存储了系统的用户权限信息。 
test：系统自动创建的测试数据库，任何用户都可以使用。
*/
```



在查看了系统中已有的数据库后，可以用如下命令选择要操作的数据库：

例如，选择数据库test1:

```mysql
mysql> USE test1;

Database changed
```

然后再用一下命令来查看数据库中创建的所有数据表：

```mysql
mysql> SHOW tables;

Empty set (0.00 sec)
```



2、删除数据库

例如，删除test1数据库：

```mysql
mysql> DROP database test1;
Query OK, 0 rows affected (0.00 sec)

/*
数据库删除后，下面的所有表数据都会全部删除，所以删除前一定要仔细检查并做好相
应备份.
*/
```



3、创建表

在数据库中创建表的基本语法如下：

```mysql
CREATE TABLE

tablename (column_name_1 column_type_1 constraints，

column_name_2

column_type_2

constraints

，

……column_name_n

column_type_n

constraints）
```

例如，创建一个名称为 emp 的表。表中包括 3 个字段，ename（姓名），hiredate（雇用日期）、 sal（薪水），字段类型分别为 varchar（10）、date、int（2）（关于字段类型将会在下一章中介绍）：

```mysql
mysql> CREATE table emp(ename varchar(10),hiredate date,sal decimal(10,2),deptno int(2));

Query OK, 0 rows affected (0.02 sec)

mysql> DESC emp; --查看emp表的基本信息

mysql> SHOW create table emp \G; 
/*更详细地显示表信息；“\G”选项的含义是使得记录能够按照字段竖着排列，对于内 容比较长的记录更易于显示。*/

```



4、删除表

例如，要删除数据库 emp 可以使用以下命令：

```mysql
mysql> DROP table emp;

Query OK, 0 rows affected (0.00 sec)
```



5、修改表

1）修改表类型，语法如下：

```mysql
ALTER TABLE tablename MODIFY [COLUMN] column_definition [FIRST | AFTER col_name]
```

例如，修改表 emp 的 ename 字段定义，将 varchar(10)改为 varchar(20)：

```mysql
mysql> ALTER table emp modify ename varchar(20);

Query OK, 0 rows affected (0.03 sec)
```



2）增加表字段，语法如下：

```mysql
ALTER TABLE tablename ADD [COLUMN] column_definition [FIRST | AFTER col_name]
```

例如，表 emp 上新增加字段 age，类型为 int(3)：

```mysql
mysql> ALTER table emp ADD column age int(3);

Query OK, 0 rows affected (0.03 sec)

Records: 0

Duplicates: 0

Warnings: 0
```



3）删除表字段，语法如下：

```mysql
ALTER TABLE tablename DROP [COLUMN] col_name
```

例如，将字段age删除掉：

```mysql
mysql> ALTER table emp DROP column age;

Query OK, 0 rows affected (0.04 sec)

Records: 0

Duplicates: 0

Warnings: 0
```



4）字段改名，语法如下：

```mysql
ALTER TABLE tablename CHANGE [COLUMN] old_col_name column_definition[FIRST|AFTER col_name]
```

例如，将 age 改名为 age1，同时修改字段类型为 int(4)：

```mysql
mysql> ALTER table emp CHANGE age age1 int(4) ;

Query OK, 0 rows affected (0.02 sec)

Records: 0

Duplicates: 0

Warnings: 

/*
change 和 modify 都可以修改表的定义，不同的是 change 后面需要写两次列名，不方便。

但是 change 的优点是可以修改列名称，modify 则不能。
*/
```



5) 修改字段排列顺序。

前面介绍的的字段增加和修改语法（ADD/CNAHGE/MODIFY）中，都有一个可选项 first|after column_name，这个选项可以用来修改字段在表中的位置，默认 ADD 增加的新字段是加在表的最后位置，而 CHANGE/MODIFY 默认都不会改变字段的位置。

例如，将新增的字段 birth date 加在 ename 之后：

```mysql
mysql> ALTER table emp ADD birth date after ename;

Query OK, 0 rows affected (0.03 sec)

Records: 0

Duplicates: 0

Warnings: 0
```

修改字段 age，将它放在最前面：

```mysql
mysql> ALTER table emp MODIFY age int(3) first;

Query OK, 0 rows affected (0.03 sec)

Records: 0

Duplicates: 0

Warnings: 0
```



6）表改名，语法如下：

```mysql
ALTER TABLE tablename RENAME [TO] new_tablename
```

例如，将表 emp 改名为 emp1，命令如下：

```mysql
mysql> ALTER table emp RENAME emp1;

Query OK, 0 rows affected (0.00 sec)
```





**2.2.3 DML语句**

DML 操作是指对数据库中表记录的操作，主要包括表记录的插入（insert）、更新（update）、删除（delete）和查询（select），是开发人员日常使用最频繁的操作。下面将依次对它们进行介绍。



1、插入记录

基本语法如下：

```mysql
INSERT INTO tablename (field1,field2,……fieldn) VALUES(value1,value2,……valuesn);
```



例如，向表 emp 中插入以下记录：ename 为 zzx1，hiredate 为 2000-01-01，sal 为 2000，deptno 为 1，命令执行如下：

```mysql
mysql> INSERT INTO emp (ename,hiredate,sal,deptno) VALUES('zzx1','2000-01-01','2000',1);

Query OK, 1 row affected (0.00 sec)
```



也可以不用指定字段名称，但是 values 后面的顺序应该和字段的排列顺序一致：

```mysql
mysql> INSERT INTO emp

VALUES('lisa','2003-02-01','3000',2);

Query OK, 1 row affected (0.00 sec)
```



在 MySQL 中，insert 语句还有一个很好的特性，可以一次性插入多条记录，语法如下：

```mysql
INSERT INTO tablename (field1, field2,……fieldn) VALUES

(record1_value1, record1_value2,……record1_valuesn),

(record2_value1, record2_value2,……record2_valuesn),

……

(recordn_value1, recordn_value2,……recordn_valuesn)

;
/*可以看出，每条记录之间都用逗号进行了分隔。*/
```



2、更新记录



对于表里的记录值，可以通过 update 命令进行更改，语法如下：

```mysql
UPDATE tablename SET field1=value1，field2.=value2，……fieldn=valuen [WHERE CONDITION]
```



例如，将表 emp 中 ename 为“lisa”的薪水（sal）从 3000 更改为 4000：

```mysql
mysql> UPDATE emp SET sal=4000 WHERE ename='lisa';

Query OK, 1 row affected (0.00 sec)

Rows matched: 1

Changed: 1

Warnings: 0
```



在 MySQL 中，update 命令可以同时更新多个表中数据，语法如下：

```mysql
UPDATE t1,t2…tn SET t1.field1=expr1,tn.fieldn=exprn

[WHERE CONDITION]
```



在下例中，同时更新表 emp 中的字段 sal 和表 dept 中的字段 deptname：

```mysql
mysql>UPDATE emp a,dept b SET a.sal=a.sal*b.deptno,b.deptname=a.ename WHERE a.deptno=b.deptno;

Query OK, 3 rows affected (0.04 sec)

Rows matched: 5

Changed: 3

Warnings: 0

/*多表更新的语法更多地用在了根据一个表的字段，来动态的更新另外一个表的字段*/
```



3、删除记录

如果记录不再需要，可以用 delete 命令进行删除，语法如下：

```mysql
DELETE FROM tablename [WHERE CONDITION]
```

例如，在 emp 中将 ename 为‘dony’的记录全部删除，命令如下：

```mysql
mysql> DELETE FROM emp WHERE ename='dony';

Query OK, 1 row affected (0.00 sec)
```



在 MySQL 中可以一次删除多个表的数据，语法如下：

```mysql
DELETE t1,t2…tn FROM t1,t2…tn [WHERE CONDITION]
```

如果 from 后面的表名用别名，则 delete 后面的也要用相应的别名，否则会提示语法错误。 在下例中，将表 emp 和 dept 中 deptno 为 3 的记录同时都删除：

```mysql
mysql> DELETE a,b FROM emp a,dept b WHERE a.deptno=b.deptno AND a.deptno=3;

Query OK, 2 rows affected (0.04 sec)

/*不管是单表还是多表，不加 where 条件将会把表的所有记录删除，所以操作时一定要小心。*/
```





4、查询记录

查询最简单的方式是将记录全部选出:

```mysql
SELECT * FROM tablename [WHERE CONDITION]
```



其中“*”表示要将所有的记录都选出来，也可以用逗号分割的所有字段来代替，例如，以下两个查询是等价的：

```mysql
mysql> SELECT * FROM emp;
mysql> SELECT ename,hiredate,sal,deptno FROM emp;

/*
“*”的好处是当需要查询所有字段信息时候，查询语句很简单，但是要只查询部分字段的 时候，必须要将字段一个一个列出来。
*/
```



1)查询不重复的记录

有时需要将表中的记录去掉重复后显示出来，可以用 distinct 关键字来实现：

```mysql
mysql> SELECT DISTINCT deptno FROM emp;
```



2）条件查询

在很多情况下，用户并不需要查询所有的记录，而只是需要根据限定条件来查询一部分数据， 用 where 关键字可以来实现这样的操作。 

例如，需要查询所有 deptno 为 1 的记录：

```mysql
mysql> SELECT * FROM emp WHERE deptno=1;
```



where 后面的条件除了‘=’外，还可以使用>、<、>=、<=、!=等比较运算符；多个条件之间还可以使用 or、and 等逻辑运算符进行多条件联合查询。

以下是一个使用多字段条件查询的例子：

```mysql
mysql> SELECT * FROM emp WHERE deptno=1 AND sal<3000;
```



3）排序和限制

用关键字 ORDER BY 来实现数据库的排序操作，语法如下：

```mysql
SELECT * FROM tablename [WHERE CONDITION] [ORDER BY field1 [DESC|ASC] ， field2 [DESC|ASC]，……fieldn [DESC|ASC]]
```

例如，把 emp 表中的记录按照工资高低进行显示：

```mysql
mysql> SELECT * FROM emp ORDER BY sal;
/*如果排序字段的值一样，则值相同的字段按照第二个排序字段进行排序，以此类推。如果只 有一个排序字段，则这些字段相同的记录将会无序排列。*/
```

 

对于排序后的记录，如果希望只显示一部分，而不是全部，这时，就可以使用 LIMIT 关键字 来实现，LIMIT 的语法如下：

```mysql
SELECT ……[LIMIT offset_start,row_count]
/*
其中 offset_start 表示记录的起始偏移量，row_count 表示显示的行数。 在默认情况下，起始偏移量为0，只需要写记录行数就可以，这时候，显示的实际就是前n条记录
*/
```

例如，显示 emp 表中按照 sal 排序后的前 3 条记录：

```mysql
mysql> SELECT * FROM emp ORDER BY sal LIMIT 3;
```

如果要显示 emp 表中按照 sal 排序后从第二条记录开始，显示 3 条记录：

```mysql
mysql> SELECT * FROM emp ORDER BY sal LIMIT 1,3;
```





**4）聚合**

很多情况下，我们需要进行一些汇总操作，比如统计整个公司的人数或者统计每个部门的人数，这个时就要用到 SQL 的聚合操作。语法如下：

```mysql
SELECT [field1,field2,……fieldn] fun_name

FROM tablename

[WHERE where_contition]

[GROUP BY field1,field2,……fieldn

[WITH ROLLUP]]

[HAVING where_contition]

/*
fun_name 表示要做的聚合操作，也就是聚合函数，常用的有 sum（求和）、count(*)（记录数）、max（最大值）、min（最小值）。 
GROUP BY 关键字表示要进行分类聚合的字段，比如要按照部门分类统计员工数量，部门就应该写在 group by 后面。 
WITH ROLLUP 是可选语法，表明是否对分类聚合后的结果进行再汇总。 
HAVING 关键字表示对分类后的结果再进行条件的过滤。
*/
```



*注意：having 和 where 的区别在于 having 是对聚合后的结果进行条件的过滤，而 where 是在聚合前就对记录进行过滤，如果逻辑允许，我们尽可能用 where 先过滤记录，这样因为结果集减小，将对聚合的效率大大提高，最后再根据逻辑看是否用 having 进行再过滤。



例如，要 emp 表中统计公司的总人数：

```mysql
mysql> SELECT COUNT(1) FROM emp;
+----------+

| count(1) |

+----------+

|        4 |

+----------+

1 row in set (0.00 sec)
```



在此基础上，要统计各个部门的人数：

```mysql
mysql> SELECT deptno,COUNT(1) FROM emp GROUP BY deptno WITH ROLLUP;
+--------+----------+
| deptno | count(1) |
+--------+----------+
|      1 |        2 |
|      2 |        1 |
|      4 |        1 |
|   NULL |        4 |
+--------+----------+

4 rows in set (0.00 sec)
```



统计人数大于 1 人的部门：

```mysql
mysql> SELECT deptno,COUNT(1) FROM emp GROUP BY deptno HAVING COUNT(1)>1;

+--------+----------+

| deptno | count(1) |

+--------+----------+

|      1 |        2 |

+--------+----------+

1 row in set (0.00 sec)
```



最后统计公司所有员工的薪水总额、最高和最低薪水：

```mysql
mysql> select * from emp;

+--------+------------+---------+--------+

| ename  | hiredate   | sal     | deptno |

+--------+------------+---------+--------+

| zzx    | 2000-01-01 | 100.00  |      1 |

| lisa   | 2003-02-01 | 400.00  |      2 |

| bjguan | 2004-04-02 | 100.00  |      1 |

| dony   | 2005-02-05 | 2000.00 |      4 |

+--------+------------+---------+--------+

4 rows in set (0.00 sec)

mysql> select sum(sal),max(sal),min(sal) from emp;

+----------+----------+----------+

| sum(sal) | max(sal) | min(sal) |

+----------+----------+----------+

| 2600.00  | 2000.00  | 100.00   |

+----------+----------+----------+

1 row in set (0.00 sec)
```





**5）表连接**

当需要同时显示多个表中的字段时，就可以用表连接来实现这样的功能。 从大类上分，表连接分为内连接和外连接，它们之间的最主要区别是**內连接仅选出两张表中 互相匹配的记录，而外连接会选出其他不匹配的记录。我们最常用的是内连接。 **



例如，查询出所有雇员的名字和所在部门名称，因为雇员名称和部门分别存放在表 emp 和 dept 中，因此，需要使用表连接来进行查询：

```mysql
mysql> SELECT * FROM emp;

+--------+------------+---------+--------+

| ename  | hiredate   | sal     | deptno |

+--------+------------+---------+--------+

| zzx    | 2000-01-01 | 2000.00 | 1      |
| lisa   | 2003-02-01 | 4000.00 | 2      |
| bjguan | 2004-04-02 | 5000.00 | 1
| bzshen | 2005-04-01 | 4000.00 | 3      |

+--------+------------+---------+--------+

4 rows in set (0.00 sec)

mysql> SELECT * FROM dept;

+--------+----------+

| deptno | deptname |

+--------+----------+

| 1      | tech     |

| 2      | sale     |

| 3      | hr       |

+--------+----------+

3 rows in set (0.00 sec)

mysql> SELECT ename,deptname FROM emp,dept WHERE emp.deptno=dept.deptno;

+--------+----------+

| ename  | deptname |

+--------+----------+

| zzx    | tech     |

| lisa   | sale     |

| bjguan | tech     |

| bzshen | hr       |

+--------+----------+

4 rows in set (0.00 sec)
```



外连接有分为左连接和右连接，具体定义如下。 

左连接：包含所有的左边表中的记录甚至是右边表中没有和它匹配的记录。 

右连接：包含所有的右边表中的记录甚至是左边表中没有和它匹配的记录。

例如，查询 emp 中所有用户名和所在部门名称：

```mysql
mysql> SELECT * FROM emp;

+--------+------------+---------+--------+

| ename  | hiredate   | sal     | deptno |

+--------+------------+---------+--------+

| zzx    | 2000-01-01 | 2000.00 | 1      |

| lisa   | 2003-02-01 | 4000.00 | 2      |

| bjguan | 2004-04-02 | 5000.00 | 1      |

| bzshen | 2005-04-01 | 4000.00 | 3      |

| dony   | 2005-02-05 | 2000.00 | 4      |

+--------+------------+---------+--------+

5 rows in set (0.00 sec)

mysql> SELECT * FROM dept;

+--------+----------+

| deptno | deptname |
+--------+----------+

| 1      | tech     |

| 2      | sale     |

| 3      | hr       |

+--------+----------+

3 rows in set (0.00 sec)

mysql> SELECT ename,deptname FROM emp LEFT JOIN dept

on emp.deptno=dept.deptno;

+--------+----------+

| ename  | deptname |

+--------+----------+

| zzx    | tech     |

| lisa   | sale     |

| bjguan | tech     |

| bzshen | hr       |

| dony   |          |

+--------+----------+

5 rows in set (0.00 sec)
```



**6）子查询**

某些情况下，当我们查询的时候，需要的条件是另外一个 select 语句的结果，这个时候，就要用到子查询。用于子查询的关键字主要包括 in、not in、=、!=、exists、not exists 等。 例如，从 emp 表中查询出所有部门在 dept 表中的所有记录：

```mysql
mysql> SELECT * FROM emp;

+--------+------------+---------+--------+

| ename  | hiredate   | sal     | deptno |

+--------+------------+---------+--------+

| zzx    | 2000-01-01 | 2000.00 | 1      |
| lisa   | 2003-02-01 | 4000.00 | 2      |
| bjguan | 2004-04-02 | 5000.00 | 1      |
| bzshen | 2005-04-01 | 4000.00 | 3      |
| dony   | 2005-02-05 | 2000.00 | 4      |

+--------+------------+---------+--------+

5 rows in set (0.00 sec)


mysql> SELECT * FROM dept;

+--------+----------+

| deptno | deptname |

+--------+----------+

| 1      | tech     |
| 2      | sale     |
| 3      | hr       |
| 5      | fin      |

+--------+----------+

4 rows in set (0.00 sec)

mysql> SELECT * FROM emp WHERE deptno IN(SELECT deptno FROM dept);

+--------+------------+---------+--------+

| ename  | hiredate   | sal     | deptno |

+--------+------------+---------+--------+

| zzx    | 2000-01-01 | 2000.00 | 1      |
| lisa   | 2003-02-01 | 4000.00 | 2      |
| bjguan | 2004-04-02 | 5000.00 | 1      |
| bzshen | 2005-04-01 | 4000.00 | 3      |

+--------+------------+---------+--------+

4 rows in set (0.00 sec)
```



某些情况下，子查询可以转化为表连接，例如：

```mysql
mysql> SELECT * FROM emp WHERE deptno IN(select deptno from dept);
+--------+------------+---------+--------+
| ename  | hiredate   | sal     | deptno |
+--------+------------+---------+--------+
| zzx    | 2000-01-01 | 2000.00 | 1      |
| lisa   | 2003-02-01 | 4000.00 | 2      |
| bjguan | 2004-04-02 | 5000.00 | 1      |
| bzshen | 2005-04-01 | 4000.00 | 3      |
+--------+------------+---------+--------+

4 rows in set (0.00 sec)

/*转换为表连接后：*/

mysql> SELECT emp.* FROM emp ,dept WHERE emp.deptno=dept.deptno;

+--------+------------+---------+--------+

| ename  | hiredate   | sal     | deptno |

+--------+------------+---------+--------+

| zzx    | 2000-01-01 | 2000.00 | 1      |
| lisa   | 2003-02-01 | 4000.00 | 2      |
| bjguan | 2004-04-02 | 5000.00 | 1      |
| bzshen | 2005-04-01 | 4000.00 | 3      |
+--------+------------+---------+--------+

4 rows in set (0.00 sec)

/*
子查询和表连接之间的转换主要应用在两个方面：
1)MySQL 4.1 以前的版本不支持子查询，需要用表连接来实现子查询的功能
2)表连接在很多情况下用于优化子查询
*/



```



**7）记录联合**

我们经常会碰到这样的应用，将两个表的数据按照一定的查询条件查询出来后，将结果合并 到一起显示出来，这个时候，就需要用 union 和 union all 关键字来实现这样的功能，具体语法如下：

```Mysql
SELECT * FROM t1

UNION|UNION ALL

SELECT * FROM t2

……

UNION|UNION ALL

SELECT * FROM tn;
```

UNION 和 UNION ALL 的主要区别是 UNION ALL 是把结果集直接合并在一起，而 UNION 是将 UNION ALL 后的结果进行一次 DISTINCT，去除重复记录后的结果。 

来看下面例子，将 emp 和 dept 表中的部门编号的集合显示出来：

```Mysql
mysql> SELECT * FROM emp;

+--------+------------+---------+--------+

| ename  | hiredate   | sal     | deptno |

+--------+------------+---------+--------+

| zzx    | 2000-01-01 | 100.00  | 1      |

| lisa   | 2003-02-01 | 400.00  | 2      |

| bjguan | 2004-04-02 | 100.00  | 1      |

| dony   | 2005-02-05 | 2000.00 | 4      |

+--------+------------+---------+--------+

4 rows in set (0.00 sec)


mysql> SELECT * FROM dept;
+--------+----------+

| deptno | deptname |

+--------+----------+

| 1      | tech     |

| 2      | sale     |

| 5      | fin      |

+--------+----------+

3 rows in set (0.00 sec)


mysql> SELECT deptno FROM emp

-> UNION ALL

-> SELECT deptno FROM dept;

+--------+

| deptno |

+--------+

| 1      |

| 2      |

| 1      |

| 4      |

| 1      |

| 2      |

| 5      |

+--------+

7 rows in set (0.00 sec)
```



如果希望将结果去掉重复记录后显示：

```Mysql
mysql> SELECT deptno FROM emp

-> UNION

-> SELECT deptno FROM dept;

+--------+

| deptno |

+--------+

| 1      |

| 2      |

| 4      |

| 5      |

+--------+

4 rows in set (0.00 sec)
```





**2.2.4 DCL语句**

DCL 语句主要是 DBA 用来管理系统中的对象权限时所使用，一般的开发人员很少使用。



下面通过一个例子来简单说明一下。 创建一个数据库用户 z1，具有对 sakila 数据库中所有表的 SELECT/INSERT 权限：

```mysql
mysql> GRANT SELECT,INSERT ON sakila.* TO 'z1'@'localhost' identified by '123';

Query OK, 0 rows affected (0.00 sec)

mysql> exit

Bye  

[mysql@db3 ~]$ mysql -uz1 -p123

Welcome to the MySQL monitor.

Commands end with ; or \g.

Your MySQL connection id is 21671 to server version: 5.1.9-beta-log
```

