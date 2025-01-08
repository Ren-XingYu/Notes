# SQL

Structured Query Language, 是一门操作关系型数据库的编程语言。SQL存在"方言"。

SQL语句以 `;`  结尾，SQL语句不区分大小写。

注释：

- 单行注释：`-- 注释内容 或 #注释内容(MySql特有)`
- 多行注释：`/* 注释 */`

# SQL分类

```sql
# 查看数据库支持的字符集
SELECT * FROM information_schema.CHARACTER_SETS;
SELECT * FROM information_schema.CHARACTER_SETS WHERE CHARACTER_SET_NAME='utf8mb4';
SHOW CHARACTER SET;
SHOW CHARACTER SET LIKE 'utf%';

# 查看字符集支持的排序规则
SELECT * FROM information_schema.COLLATIONS;
SELECT * FROM information_schema.COLLATIONS WHERE CHARACTER_SET_NAME='utf8mb4';
SHOW COLLATION;
SHOW COLLATION WHERE Charset = 'utf8mb4';

# 查看数据库当前使用的字符集
SHOW VARIABLES;
SHOW VARIABLES WHERE Variable_name LIKE 'character_set%';
SHOW VARIABLES LIKE 'character_set%';

# 查看数据库当前使用的字符比较规则
SHOW VARIABLES LIKE '%collation%'

# 查看字符序(每个字符集对应一个或多个字符序)
# bin: 二进制
# ci: 大小写不敏感
# cs: 大小写敏感
# ai: 口音(Accent)不敏感
# as：口音(Accent)敏感
# ks: 假名(Kanatype)敏感
show collation;
show collation where Charset='utf8mb4';
```

## DDL

Data Definition Language，用来定义数据库对象：数据库，表列等

```mysql
show databases;

create database ssm;
create database if not exists ssm;

drop database ssm;
drop database if exists ssm;

# 显示当前正在使用的数据库
select database();

use ssm;

#########################

show tables;

desc tb_user;

create table tb_user(
	id int,
	username varchar(20),
	password varchar(20)
);

drop table tb_user;
drop table if exists tb_user;

insert into tb_user values(1,'abc','xyz');

select * from tb_user;

alter table tb_user rename to t_user;
alter table t_user add scope double(5,2);

select * from t_user;

desc t_user;

alter table t_user modify username varchar(30);
alter table t_user change password pwd varchar(30);
alter table t_user drop scope;
```

## DML

Data Manipulation Language，用来对数据库中表的数据进行增删改

```sql
select * from t_user;

insert into t_user(id,username,pwd) values(1,'aaa','bbb');
insert into t_user(id,username,pwd) values(2,'ccc','ddd'),(3,'eee','fff');

update t_user set username='ggg' where id=3;

delete from t_user;
```

## DQL

Data Query Language，用来查询数据库中表的记录(数据)

一个完整的SQL查询如下：

```sql
SELECT DISTINCT column, AGG_FUNC(column_or_expression), …
FROM mytable
    JOIN another_table
      ON mytable.column = another_table.column
    WHERE constraint_expression
    GROUP BY column
    HAVING constraint_expression
    ORDER BY column ASC/DESC
    LIMIT count OFFSET COUNT;
```

SQL关键字执行顺序：

- **FROM and JOIN**

  从`from`和`join`关键字得到要查询的表，比如这里是`mytable`和`another_table`，对这两个表进行笛卡儿积运算，生成临时表`T1`

- **ON**

  对临时表`T1`进行`ON`条件过滤生成临时表`T2`

- **OUTER，添加外连接行**

  这一步只有在连接为`outer join`时才会发生，这里的工作主要是在临时表`T2`的基础上添加保留表中被过滤条件过滤掉的数据，非保留表中的数据被赋予`NULL`值，最后生成临时表`T3`

- **WHERE**

  对临时表`T3`进行`WHERE`条件过滤，生成临时表`T4`

- **GROUP BY**

  对临时表`T4`进行分组操作，生成临时表`T5`

- **HAVING**

  对临时表`T5`进行`HAVING`条件过滤，生成临时表`T6`

- **SELECT**

  对临时表`T6`进行`SELECT`操作，生成临时表`T7`

  **注意：** 这里有一个需要注意的点是别名的使用，如下SQL语句

  	select customer_id cid,email e,address a
  	from customer c
  	where c.cid='xxx'

  由上可知该句SQL的执行顺序为`from-->where-->select`，由于`from`在`where`前面执行，所以可以在`where`中使用`表的别名`，由于`select`在`where`的后面执行，所以不能在`where`中使用`列的别名`。该句SQL的正确写法为:

  	select customer_id cid,email e,address a
  	from customer c
  	where c.customer_id='xxx'

- **DISTINCT**

  对临时表`T7`进行`DISTINCT`操作，生成临时表`T8`

- **ORDER BY**

  对临时表`T8`进行`ORDER BY`操作，生成临时表`T9`

- **LIMIT / OFFSET**

  对临时表`T9`进行`LIMIT`或`OFFSET`操作得到指定位置的数据，并返回结果集

```sql
# where 条件
>
<
>=
<=
=
<>或!=
between ... and ...
in ()
like '%XXX_' # %:0个或多个字符 _:单个任意字符
is null 
is not null
and 或 &&
or 或 ||
not 或 !

# order by
asc(默认)
desc

# 聚合函数(null值不参与聚合函数的运算)
count
max
min
sum
avg

# 分组查询(where分组前限定,having分组后限定)
group by xxx having xxx

# 分页查询
limit 起始索引(0开始),查询的条数
limit m,n
起始索引=(m-1)*n
# limit是mysql的方言,oracle的分页是rownumber,sql server的分页是top

select * from t_user;

select distinct username from t_user;

select * from t_user where username is not null;

select * from t_user order by id desc;
```

## DCL

Data Control Language，用来定义数据库的访问权限和安全级别，及创建用户

# 数据类型

## 数值

```
TINYINT			 1 byte
SMALLINT		 2 byte
MEDIUMINT		 3 byte
INT或INTEGER		4 byte
BIGINT			 8 byte
FLOAT			 4 byte
DOUBLE			 8 byte
DECIMAL
```

## 日期

```
DATE		3
TIME		3
YEAR		1
DATETIME	8
TIMESTAMP	4
```

## 字符串

```
CHAR		0-255 byte
VARCHAR		0-65535 byte
TINYBLOB	0-255 byte
TINYTEXT	0-255 byte
BLOB		0-65535 byte
TEXT		0-65535 byte
MEDIUMBLOB	0-16777215 byte
MEDIUMTEXT	0-16777215 byte
LONGBLOB	0-4294967295 byte
LONGTEXT	0-4294967295 byte
```



# 约束

作用于表中列上的规则，保证数据的正确性、有效性、完整性

```mysql
# 非空约束
not null

# 唯一约束
unique

# 主键约束(非空且唯一)
primary key

# 默认约束
default

# 检查约束(mysql不支持)
check

# 外键约束
foreign key
alter table 表名 add constraint 外键名称 foreign key(外键字段名称) references 主表名称(主表列名称);
alter table 表名 drop foreign key 外键名称;
```

# 多表查询

## 连接查询

### 内连接

```sql
# 隐式内连接
select xxx from table1,table2 where xxx;

# 显式内连接
select xxx from table1 [inner] join table2 on xxx;
```

### 外连接

```sql
# 左外连接
select xxx from table1 left [outer] join table2 on xxx;

# 右外连接
select xxx from table1 right [outer] join table2 on xxx;
```

## 子查询

```sql
# 单行单列
select xxx from table1 where xxx = (子查询);

# 多行单列
select xxx from table1 where in = (子查询);

# 多行多列
select xxx from (子查询) where xxx;
```

# 事务

```sql
# 开启事务
start transaction; 或者 begin;
# 提交事务
commit;
# 回滚事务
rollback;

# 查看事务的默认提交级别
select @@autocommit;

# 0:手动提交 1:自动提交
set @@autocommit=1;
```

事务的四大特征：

- 原子性(Atomicity)：事务是不可分割的最小操作单元，要么同时成功，要么同时失败。
- 一致性(Consistency)：事务完成时，必须所有的数据都保持一致状态。
- 隔离性(Isolation)：事务之间相互隔离，多个事务之间，操作的可见性。
- 持久性(Durability)：事务一旦提交或回滚，它对数据库中数据的改变是永久的。

# 预编译SQL

```
PreparedStatement预编译功能开启：userServerPrepStmts=true
```

# 连接池

```
标准接口：DataSource

常见连接池：
	DBCP
	C3P0
	Druid
```

