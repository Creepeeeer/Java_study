# 	基础

**Mysql启动**

```java
net start mysql80
net stop mysql80
```



**Mysql客户端连接**

```java
mysql [-h 127.0.0.1] [-P 3306] -u root -p
```



## SQL

### 通用语法和分类

#### SQl通用语法

- SQL语句以分号结尾
- MYSQL数据库的SQL语句不区分大小写，关键字建议大写
- 单行注释 `--`,`#`
- 多行注释 `/**/`

#### SQL分类

- DDL 数据定义语言，用来定义数据库对象(数据库，表，字段)
- DML 数据操作语言，用来对数据库表中的数据进行增删改
- DQL 数据查询语言，用来查询数据库中表的记录
- DCL 数据控制语言，用来创建数据库用户，控制数据库的访问权限



### DDL

#### DDL 数据库操作

```sql
SHOW DATABASES;//查询所有数据库
SELECT DATABASE();//查询当前数据库
CREATE DATABASE[IF NOT EXISTS]数据库名[DEFAULT CHARSET字符集][COLLATE 排序规则];//创建
//如果已经存在了再创建会报错 除非加if not exists就不会报错了
//字符集推荐用utf8mb4
DROP DATABASE [IF EXIST]数据库名;//删除
USE 数据库名;//使用
```



#### DDL表操作

**查询**

```sql
SHOW TABLES;//查询当前数据库所有表，前提是先进入一个数据库
DESC 表名;//查询表结构
SHOW CTREATE TABLE 表名;//查询指定表的建表语句
```

**创建**

```sql
CTREATE TABLE 表名(
 字段1 字段1类型[COMMENT 字段1注释],
 字段2 字段2类型[COMMENT 字段2注释],
 .....
 字段n 字段n类型[COMMENT 字段n注释],
)[COMMENT 表注释];
# [...]为可选参数 最后一个字段没有逗号
#e.g
mysql> create table tb_user(
    -> id int comment '编号',
    -> name varchar(50) comment '名字',
    -> age int comment '年龄'
    -> ) comment '用户表';
```

**数据类型**

主要分为三类：数值类型，字符串类型，日期时间类型

数值类型

```sql
tinyint 1 byte
smallint 2 byte
mediumint 3 byte
int 4 byte
bigint 8 byte
float 4 byte
double 8 byte
decimal 精确定点数
#e.g
age tinyint unsigned
score double(4,1) //整数最长位数+小数最长位数  小数最长位数
```

字符串类型

```sql
char 定长字符串，空间是固定的规定是多少就是多少，性能好 char(10) 
varchar 变长字符串 性能较差 varchar(10) 
```

日期类型

```sql
date YYYY-MM-DD #年月日
time HH:MM:SS #时分秒
year #年份
datetime #年月日时分秒 
```



**修改**

```sql
alter table 表名 add 字段名 类型(长度) [comment 注释] [约束];#添加字段
alter table 表名 modify 字段名 新的数据类型(长度);#修改数据类型
alter table 表名 change 旧字段名 新字段名 类型(长度) [comment 注释] [约束];#修改字段名和数据类型
alter table 表名 drop 字段名;#删除字段
alter table 表名 rename to 新表名;#修改表名
```

**删除**

```sql
drop table [if exists]表名;#删除表
truncate table 表名;#删除指定表并重新创建该表
```



### DML

#### 添加数据

```sql
insert into 表名(字段名1,字段名2,...) values(值1,值2,...);#给指定字段添加数据
insert into 表名 values(值1,值2,...);#给全部字段添加数据
insert into 表名(字段名1,字段名2,..) values(值1，值2,...),(值1，值2,...)...（值1，值2，...）;
insert into values(值1，值2,...),(值1，值2,...)...（值1，值2，...）;
```

ps:

- 字符串和日期型数据应该包含在单引号中



#### 修改数据

```sql
update 表名 set 字段名1 =值1，字段名2=值2,...[where 条件];
e.g
update emp set name='333' where id=1;
```



#### 删除数据

```sql
delete from 表名 [where 条件];
```



### DQL

```sql
select 
	字段列表
from
	表名列表
where
	条件列表
group by
	分组字段列表
having
	分组后条件列表
order by
	排序字段列表
limit
	分页参数
```

#### 基本查询

```sql
select 字段1,字段2,字段3... from 表名;//查询多个字段
select * from 表名;//查询全部字段
select 字段1[as 别名1],字段2[as 别名2].... from 表名;//设置别名
select distinct 字段列表 from 表名;//去重重复记录
```

#### 条件查询

```sql
select 字段列表 from 表名 where 条件列表;
//比较运算符
>,>=,<,<=,=,!=
between...and... #左闭右闭区间
in(...) 在in之后列表中的值，多选1
like 占位符 模糊匹配(_匹配单个字符,%匹配任意个字符)
is null 是null
//逻辑运算符
and或&& or或|| not或!
e.g 
select * form emp where id is not null;
select * from emp where id in(1,2,3);
select * from emp where name like '__';//name为两个字
select * from emp where idcard like '%X';//身份证号最后一位为X
```

#### 聚合函数

将一列数据作为一个整体进行纵向计算

```sql
select 聚合函数(字段列表) from 表名;
//常见聚合函数
count 统计数量
max 最大值
min 最小值
avg 平均值
sum 求和
//null值不参与聚合函数计算
```

#### 分组查询

**分组一般配合着聚合函数使用**

```sql
select 字段列表 from 表名 where 条件 group by 分组字段名 having 分组后过滤条件;
#where是分组前进行过滤，having是分组后进行过滤
e.g
select gender,count(*) from emp group by gender;# 根据性别分组，统计男性员工和女性员工的数量
select gender,avg(age) from emp group by gender;# 根据性别分组，统计男性员工和女性员工的平均年龄
select workaddress,count(*) from emp where age<45 group by workaddress having count(*)>=3;
#查询年龄小于45的员工，并根据工作地址分组，获取员工数量>=3的工作地址
```

#### 排序查询

```sql
select 字段列表 from 表名 order by 字段1 排序方式1,字段2 排序方式2；
#排序方式 asc升序(默认值，可省略) desc 降序
#如果是多字段排序，当第一个字段值相同的时候，才会根据第二个字段进行排序
#e.g
select * from emp order by age asc;#按照年龄升序
```

#### 分页查询

```sql
select 字段列表 from 表名 limit 起始索引,查询记录数;
//起始索引从0开始，起始索引=(查询页码-1)*每页显示的记录数
//分页查询是数据库的方言，不同数据库有不同的实现，MYSQL是limit
//如果查询的是第一页数据，起始索引可以省略，直接简写为limit 10	
```

#### 执行顺序

```sql
from 表名列表
where 条件列表
group by 分组字段列表
having 分组后条件列表
select 字段列表
order by 排序字段列表
limit 分页参数
```



### DCL

#### 管理用户

```sql
use mysql;
select *from user;//查询用户
create user '用户名'@'主机名' identified by '密码';//创建用户
alter user ‘用户名’@‘主机名’ identified with 旧的密码 by '新的密码';//修改用户密码
drop user ‘用户名’@'主机名';//删除用户
    //e.g
//create user ‘itcast’@'localhost' identified  by '123456';//这个用户只能被当前主机localhost访问
//create user 'aaa'@'%' identified by '123456';//可以被任何主机访问
```

#### 权限控制

**常用的权限**

- all，all privileges 所有权限
- select 查询数据
- insert 插入数据
- update 修改数据
- delete 删除数据
- alter 修改表
- drop 删除数据库/表/视图
- create 创建数据库/表

```sql
show grants for '用户名'@'主机名';//查询权限
grant 权限列表 on 数据库名.表名 to '用户名'@'主机名';//授予权限
revoke 权限列表 on 数据库名.表名 from ‘用户名’@'主机名';//撤销权限
ps: 多个权限之间使用逗号分隔
授权时，数据库名和表名可以使用*进行通配,代表所有
```



## 函数

### 字符串函数

```sql
concat(s1,s2,...sn) 将s1,s2...sn拼接成一个字符串
lower(str) 将str全部转成小写
upper(str) 将str全部转成大写
lpad(str,n,pad) 左填充，用字符串pad在str左边填充，直到str长度为n
rpad(str,n,pad) 右填充,用字符串pad在str右边填充，直到str长度为n
trim(str) 去掉字符串头部和尾部的空格
substring(str,start,len) 返回子串（mysql的字符串下标从1开始）
#使用select来执行这些函数
```



### 数值函数

```sql
ceil(x);//向上取整
floor(x);//向下取整
mod(x,y);//返回x%y
rand();//0~1随机数
round(x,y);四舍五入x保留y位小数
//e.g 生成一个6位数的随机验证码
select lpad(round(rand(),6)*1000000,6,'0');
```



### 日期函数

```sql
curdate();//返回当前日期
curtime();//返回当前时间
now();//返回当前日期和时间
year(date);//获取指定date的年份
month(date);//获取指定date的月份
day(date);//获取指定date的日期
date_add(date,interval expr type)//返回一个日期/时间值加上一个时间间隔expr后的时间值
datediff(date1,date2)//返回起始时间date1和结束时间date2之间的天数
//e.g
select year(now());
select date_add(now(),interval 70 day);//往后推70天
select datediff('2021-10-01','2021-12-01');
```



### 流程函数

```sql
if(value,t,f) //如果value为true，返回t，否则返回f
ifnull(value1,value2)//如果value1不为空返回value1，否则返回value2
case when [val1] then [res1]...else [default] end;//如果val1为true，返回res1,..否则返回default默认值
case [expr] when [val1] then [res1] ... else [default] end;//如果expr的值等于val1，返回res1，否则返回default默认值
//e.g
select id,name,
       (case when math>=85 then '优秀' when math>=60 then '及格' else '不及格'end) as '数学',
       (case when english>=85 then '优秀' when english>=60 then '及格' else '不及格'end) as '英语',
       (case when chinese>=85 then '优秀' when chinese>=60 then '及格' else '不及格'end) as '语文'
from score;
```



## 约束

### 概述

约束是作用在表上字段的规则，用来限制存储在表中的数据

**约束是作用于表中字段上的，可以在创建表/修改表的时候添加约束**

分类

| 约束     | 描述                                                     | 关键字      |
| -------- | -------------------------------------------------------- | ----------- |
| 非空约束 | 限制该字段不能为null                                     | NOT NULL    |
| 唯一约束 | 保证该字段数据都是唯一不重复的                           | UNIQUE      |
| 主键约束 | 主键是一行数据的唯一标识，要求非空且唯一                 | PRIMARY KEY |
| 默认约束 | 保存数据时，如果未指定该字段的值，将使用默认值           | DEFAULT     |
| 检查约束 | 保证字段值满足某一条件                                   | CHECK       |
| 外键约束 | 用来让两张表的数据之间建立连接，保证数据的完整性和一致性 | FOREIGN KEY |



### 约束演示

**多个约束之间用空格隔开**

```sql
create table user(
    id int primary key auto_increment comment '主键',#主键，且自动自增
    name varchar(10) not null unique,#不为空，且唯一
    age int check(age>0 &&age <=120),#大于0，小于120
    status char(1) default '1',#如果没有指定该值，默认为1
    gender char(1)
)comment '用户表';
auto_increment 字段的值不要去手动插入，sql会自己计算
```



### 外键约束

外键用来让两张表之间建立连接，从而保证数据的一致性和完整性

拥有外键的叫子表，另外一个表叫父表

```sql
create table dept(
    id int auto_increment primary key comment 'ID',
    name varchar(10) not null comment '部门名称'
);
insert into dept values(1,'市场部'),(2,'研发部'),(3,'销售部');
create table emp(
    id int auto_increment primary key ,
    name varchar(10) not null ,
    dept_id int comment '部门ID'
)comment '员工表';
insert into emp (name,dept_id) values('AAA',1),('BBB',2),('CCC',3);
```

此时dept表的id与emp表的dept_id含义相同应该建立外键约束

**添加删除外键**

```sql
create table 表名(
    字段名 数据类型,
    ...
    constraint 外键名称 foreign key(外键字段名) references 主表(主表列名)
);
alter table 表名 add constraint 外键名称 foreign key(外键字段名) references 主表(主表列名);//在表已经存在的时候添加外键
alter table 表名 drop foreign key 外键名称;//删除外键
//e.g
alter table emp add constraint fk_emp_dept_id foreign key(dept_id) references dept(id);//此时是给子表dept建立外键
//假如此时要删除父表中的(1,'市场部')这一条记录，因为子表中('AAA',1)这条记录和父表中要删除记录相关联所以不能删除
```



### 外键删除更新行为

| 行为               | 说明                                                         |
| ------------------ | ------------------------------------------------------------ |
| no action/restrict | 当在父表中删除/更新记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新 |
| cascade            | 当在父表中删除/更新记录时，首先检查该记录是否有对应外键，如果则删除/更新外键在子表中的记录 |
| set null           | 当在父表中删除对应记录时，首先检查该记录是否有对应外键，如果有则设置子表中该外键值为null |



``` sql
alter table 表名 add constraint 外键名称 foreign key(外键字段) references 主表名(主表字段名) on update cascade on delete cascade;	
//e.g
alter table emp add constraint dept_key foreign key(dept_id) references dept(id) on update cascade on delete cascade;
```



## 多表查询

### 多表关系

- 一对多（多对一）

一个部门对应多个员工，一个员工对应一个部门

在多的一方建立外键，指向一的一方的主键

- 多对多

一个学生可以选修多门课程，一门课程也可以被多名学生选修

建立第三张中间表，中间表至少包含两个外键，分别关联两方主键

```sql
create table student(
    id int auto_increment primary key comment'主键ID',
    name varchar(10) comment '姓名',
    num varchar(10) comment '学号'
);
insert into student values(null,'AAA','111'),(null,'BBB','222');
create table course(
    id int auto_increment primary key comment '主键ID',
    name varchar(10) comment'课程名称'
);
insert into course values(null,'Java'),(null,'PHP'),(null,'MYSQL');
create table student_course(
    id int auto_increment primary key comment '主键',
    studentid int not null comment '学生id',
    courseid int not null comment '课程id',
    constraint fk_studentid foreign key(studentid) references student(id),
    constraint fk_courseid foreign key(courseid) references course(id)
);
insert into student_course values(null,1,1),(null,1,2),(null,2,2),(null,2,3);
```

- 一对一

用户和用户详情的关系

多用于单表拆分，将一张表的基础字段放在一张表中，其他详情字段放在另一张表中

在任意一方加入外键，关联另一方的主键，并且设置外键是unique

```sql
create table tb_user(
    id int auto_increment primary key comment'主键',
    name varchar(10) comment'姓名',
    age int check( age>0 &&age <=100) comment '年龄'
);
create table tb_user_edu(
    id int auto_increment primary key comment'主键',
    midschool varchar(10) comment'中学',
    useid int unique comment'用户id',
    constraint fk_id foreign key(useid) references tb_user(id)
);
```



### 概述

```sql
select * from 表1,表2....；
```

多表查询是两个集合的笛卡尔积的结果，假如A表有a条数据，B表有b条数据，那么多表查询的结果为a*b条



### 内连接

查询两张表之间的交集的部分

```sql
select 字段列表 from 表1，表2 where 条件...;//隐式内连接
select 字段列表 from 表1 inner join 表2 on 连接条件...//显式内连接
//e.g
select emp.name,dept.name from emp,dept where emp.deptid=dept.id;
```



### 外连接

对于内连接 假如某个数据的dept.id为null，那么内连接的表将查询不到这条数据

- 左外连接: 查询表1(左表)的所有数据以及表1和表2交集部分的数据；

```sql
select 字段列表 from 表1 left outer join 表2 on 条件;
//e.g
select emp.*,dept.name from emp left outer join dept on emp.deptid=dept.id;
```

- 右外连接: 查询表2(右表)的所有数据以及表1和表2交集部分的数据；

```sql
select 字段列表 from 表1 right outer join 表2 on 条件;
```



### 自连接

相当于一张表自己连接自己，可以是外连接也可以是内连接

```sql
select 字段列表 from 表A 别名A join 表A 别名B on 条件;
//e.g
查询所有员工emp及其领导的名字emp
select A.name,B.name from emp A,emp B where A.managerid=B.id;//使用内连接
select A.name,B.name from emp A left outer join emp B on A.managerid=B.id;//使用外连接，假如一个员工没有领导，使用内连接不会显示，但是使用外连接会显示
```



### 联合查询

相当于将两个查询的结果合并在一起，但是两个查询的结果必须列数保持一致，字段类型也要保持一致

union all 会将全部的数据直接合并在一起，union 会对合并之后的数据进行去重

```sql
select 字段列表 from 表A...
union [all]
select 字段列表 from 表B...
```



### 子查询

SQL语句中嵌套select语句

```sql
select *from t1 where column1=(select column1 from t2);
```



#### 标量子查询

子查询的结果是单个值(数字，字符串，日期)

```sql
//e.g
//查询"销售部"的所有员工信息
select * from emp where deptid=(select id from dept where dept.name='销售部');
//查询"方东白"入职之后的员工信息
select *from emp where entrydate >(select entrydate from emp where name='方东白');
```

#### 列子查询

子查询返回的结果是一列(可以是多行)

常用的操作符

| 操作符 | 描述                                   |
| ------ | -------------------------------------- |
| in     | 在指定范围内多选1                      |
| not in | 不在指定的范围内                       |
| any    | 子查询返回列表中，有任意一个满足即可   |
| some   | 与any等同，使用some的地方都可以使用any |
| all    | 子查询返回列表的所有值都必须满足       |

```sql
//e.g
//查询‘销售部’和‘市场部’的所有员工信息
select * from emp where deptid in(select id from dept where name='销售部'or name='市场部');
//查询比财务部所有人工资都高的员工信息
select * from emp where salary > all(select salary from emp where deptid=(select id from dept where name='销售部'));
//查询比研发部其中任何一人工资高的员工信息
select * from emp where salary > any(select salary from emp where deptid=(select id from dept where name='研发部'));
```



#### 行子查询

```sql
//e.g
//查询和张无忌薪资及直属领导相同的员工信息
select * from emp where (salary,managerid)=(select salary,managerid from emp where name='张无忌');
```



#### 表子查询

```sql
//e.g
//查询与路障可和宋远桥职位和薪资相同的员工信息
select * from emp where(salary,job) in(select salary,job from emp where name='路障可'or name='宋远桥');
//查询入职日期是2006-01-01 之后的员工信息，及其部门信息
select e.*,d.* from (select * from emp where entrydate>'2006-01-01') e left outer join dept d on d.id =e.deptid;
```





## 事务

**简介**

事务是一堆操作的集合，是一个不可分割的工作单位，事务会把所有的操作作为一个整体一起向系统提交或撤销操作请求，即这些操作要么同时成功要么同时失败

默认的MySQL的事务是自动提交的，当执行一条DML语句，MYSQL会立即隐式的提交事务

### 事务操作

```sql
select @@autocommit;//查看事务提交方式：1为自动提交 0为手动提交
set @@autocommit=0;//设置事务提交方式为手动提交
start transaction;//开启事务,和上面那个等效
commit; //提交事务，将刚才的修改提交到数据库
rollback;//回滚事务 就相当于撤销刚才的修改到开启事务之前
//e.g
select @@autocommit;
set @@autocommit =1;#或者下面
start transaction;
-- 转账操作（张三给李四转1000）
-- 1.查询张三余额
    select * from account where name='张三';
-- 2.将张三余额-1000
    update account set money =money -1000 where name='张三';
-- 3.将李四余额+1000
    update account set money=money +1000 where name='李四';

commit;#如果前面执行没出错就commit，否则就rollback
rollback;
```



### 事务的四大特性(ACID)

- 原子性：事务是不可分割的最小操作单元，要么全部成功，要么全部失败
- 一致性： 事务完成时，必须使所有的数据都保持一致状态
- 隔离性 ：数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行
- 持久性 ：事务一旦提交或回滚，它对数据库的数据的改变就是永久的

### 并发事务问题

- 脏读： 一个事务读到另外一个事务还没有提交的数据
- 不可重复读：一个事务先后读取同一条记录，但两次读取的数据不同，称之为不可重复读
- 幻读：一个事务按照条件查询数据时，没有对应的数据行，但是在插入数据时，又发现这行数据语句存在，好像出现了一个幻影

### 事务隔离级别

| 隔离级别                   | 脏读 | 不可重复读 | 幻读 |
| -------------------------- | ---- | ---------- | ---- |
| Read uncommitted           | ✅    | ✅          | ✅    |
| Read commited              | :x:  | ✅          | ✅    |
| Repeatable Read(MYSQL默认) | :x:  | :x:        | ✅    |
| Serializable               | :x:  | :x:        | :x:  |

```sql
select @@transaction_isolation;//查看事务隔离级别
set [session|global] transaction isolation level {read uncommitted |read commited|repeatable read|serializable}//设置事务隔离级别
//session表示当前窗口有效，global表示对所有对话框都有效
```

**事务隔离级别越高，数据越安全，但是性能越低**







# 进阶

## 存储引擎

### MYSQL体系结构

![image-20260402221315943](C:\Users\LENOVO\AppData\Roaming\Typora\typora-user-images\image-20260402221315943.png)



### 存储引擎简介

存储引擎就是存储数据，建立索引，更新/查询数据等技术的实现方式，存储引擎是基于表的，而不是基于库的，所以存储引擎也可被称为表类型

MYSQL默认存储引擎:InnoDB

```sql
create table 表名(
    字段1 字段1类型[comment 字段1注释],
    ...
    字段n 字段n类型[comment 字段n注释]
)engine =InnoDB [comment 表注释];//在创建表时指定存储引擎
show engines;//查看当前数据库支持的存储引擎
```

### 存储引擎特点

#### InnoDB

- InnoDB是一种兼任高可靠性和高性能的通用存储引擎，在MYSQL5.5之后,InnoDB是MYSQL默认存储引擎
- DML操作遵循ACID模型，支持事务
- 行级锁，提高并发访问性能
- 支持外键FOREIGN约束，保证数据的完整性和正确性
- 文件：xxx.idb xxx代表的是表名，InnoDB引擎的每张表都会对应这样一个表空间文件，存储该表的表结构(frm,sdi)，数据和索引
- 参数:innodb_file_per_table



#### MyISAM

- MYISAM是MYSQL早期的默认存储引擎
- 不支持事务，不支持外键
- 支持表锁，不支持行锁
- 访问速度快
- xxx.sdi：存储表结构信息，xxx.MYD 存储数据 ，xxx.MYI 存储索引



#### Memory

- Memory引擎的表数据存储在内存中，由于受到硬件问题，或者断电影响，只能将这些表作为临时表或缓存使用
- 内存存放
- hash索引（默认）
- xxx.sdi 存储表结构信息



### 存储引擎选择

- InnoDB: 是MYSQL的默认存储引擎，支持事务，外键，如果应用对事务的完整性有比较高的要求，在并发条件下要求数据一致性，数据操作除了插入和查询之外，还包括很多的更新，删除操作，那么InnoDB存储引擎是比较合适的选择
- MyISAM：如果应用是以读操作和插入操作为主，只有很少的更新和删除操作，并且对事务的完整性，并发性要求不是很高，那么选择这个存储引擎是非常合适的
- MEMORY：将所有数据保存在内存中，访问速度快通常用于临时表及缓存，MEMORY的缺陷就是对表的大小有限制，太大的表无法缓存在内存中，而且无法保证数据的安全性





## 索引

索引是帮助MYSQL高效获取数据的数据结构

| 索引结构              | 描述                                                         |
| --------------------- | ------------------------------------------------------------ |
| B+Tree索引            | 最常见的索引类型，大部分引擎都支持B+树索引                   |
| Hash索引              | 底层数据结构用哈希表实现的，只有精确匹配索引列的查询才有效，不支持范围查询 |
| R-tree(空间索引)      | 空间索引是MYISAM引擎的一个特殊索引类型，主要用于地理空间数据类型，通常使用较少 |
| Full-text（全文索引） | 是一种通过建立倒排索引，快速匹配文档的方式，类似于Lucene,Solr,ES |



### 结构

- MYSQL索引数据结构对经典的B+Tree进行了优化，在原B+Tree的基础上，增加一个指向相邻子节点的链表指针
- MYSQL中支持hash索引的是Memory引擎，而InnoDB中具有自适应hash功能，hash索引是存储引擎根据B+Tree索引在指定条件下自动构建的



### 分类

| 分类     | 含义                                                 | 特点                     | 关键字   |
| -------- | ---------------------------------------------------- | ------------------------ | -------- |
| 主键索引 | 针对于表中主键创建的索引                             | 默认自动创建，只能有一个 | primary  |
| 唯一索引 | 避免同一个表中某数据列中的值重复                     | 可以有多个               | unique   |
| 常规索引 | 快速定位特定数据                                     | 可以有多个               |          |
| 全文索引 | 全文索引查找的是文本中的关键词，而不是比较索引中的值 | 可以有多个               | fulltext |

在InnoDB存储引擎中，根据索引的存储形式，又可以分为以下两种

| 分类     | 含义                                                       | 特点                 |
| -------- | ---------------------------------------------------------- | -------------------- |
| 聚集索引 | 将数据存储和索引放到了一块，索引结构的叶子节点保存了数据   | 必须有，而且只有一个 |
| 二级索引 | 将数据与索引分开存储，索引结构的叶子节点关联的是对应的主键 | 可以存在多个         |

聚集索引选取规则：

- 如果存在主键，主键索引就是就聚集索引
- 如果不存在主键，将使用第一个唯一索引作为聚集索引
- 如果没有主键也没有唯一索引，那么InnoDB将会自动生成一个rowid作为隐藏的聚集索引

可以理解成，二级索引叶子节点存的是聚集索引，然后再去聚集索引里面找数据



### 语法

```sql
create [unique|fulltext] index index_name on table_name(index_col_name,...);//创建索引
show index from table_name;//查看索引
drop index index_name on table_name;//删除索引
//e.g
create index un_idx on tb_user(profession,age,status);//为profession,age,status创建联合索引
```



### SQL性能分析

#### SQL执行频率

通过下面命令可以查看当前数据库的insert，update，delete，select的访问频次

```sql
show global status like 'Com_______';//七个下划线
```

#### 慢查询日志

慢查询日志记录了所有执行时间超过指定参数(long_query_time，单位秒，默认10s)的是所有SQL语句的日志

查询慢查询是否开启

```sql
show variables like 'slow_query_log';
```

MYSQL慢查询日志默认没有开启，需要在MYSQL的配置文件(/etc/my.cnf)中配置如下信息

```sql
slow_query_log=1 #开启慢日志查询开关
long_query_time=2 #设置慢日志的时间为2s
```

#### profile详情

show profiles能够在做SQL优化时帮助我们了解时间都耗费到哪里去了

```sql
select @@have_profiling;//查看当前MYSQL是否支持profile操作
select @@profiling //查看profiling是否开启
set profiling =1;//开启profiling

show profiles;//查看每一条SQL的耗时基本情况
show profile for query query_id//查看指定query_id的sql语句在各个阶段的耗时情况，这里的query_id通过show profiles来查询
show profile cpu for query query_id;//查看指定query_id的sql语句CPU使用情况
```

#### explain执行计划

**获取MYSQL如何执行select语句的信息，包括表如何连接和连接的顺序**

```sql
#在任何的select语句之前加上explain/desc
explain select 字段列表 from 表名 where 条件;
```

**各字段含义**

- id：select查询的序列号，表示查询中执行的select子句或者是操作表的顺序(id相同，执行顺序从上到下，id不同，值越大越先执行)
- select_type ：表示select的类型，常见的取值为simple(简单表，不适用表连接或者子查询)，primary(主查询)，union(union后的第二个或者后面的查询)，subquery(select/where之后包含了子查询)
- **type**:表示连接类型，性能由好到坏为：null>system>const>eq_ref>ref>range>index>all
- possible_key： 显示可能应用在这张表上的索引，一个或多个
- key：实际使用的索引，如果为NULL则没有使用



### 索引使用

#### 最左前缀法则

如果整个索引是多列(联合索引)，要遵循最左前缀法则，最左前缀法则指的是查询从索引的最左列开始，并且不跳过索引中的列，如果中间某一列不存在，那么后面的索引将失效

```sql
#假如存在一个索引 create index pro_age_sta_idx on tb_user(profession,age,status);
explain select *from tb_user where profession='软件工程' and age=31 and status='0';//三个索引都生效
explain select *from tb_user where status='0' and age=31 and profession='软件工程';//和上面等效，与顺序无关
explain select *from tb_user where profession='软件工程' and age=31 ;//前两个索引生效
explain select *from tb_user where profession='软件工程' and status='0';//只有第一个索引生效
explain select *from tb_user where age=31 and status='0';//三个索引都失效
```



#### 范围查询

联合索引中出现范围查询(>,<)，范围查询的右侧的索引失效

```sql
explain select *from tb_user where profession='软件工程' and age>31 and status='0';//前两个索引生效
```

但是>=或者<=不会出现这种情况，所以尽量使用>=\\<=避免索引失效



#### 索引失效

- 如果是用函数运算的结果来查找，索引将失效

```sql
explain select * from tb_user where substring(phone,10,2)='15';
```

- 如果字符串忘记加单引号索引也会失效，但是查询的结果是正确的
- 如果只是尾部模糊匹配，索引不会失效，如果是头部模糊匹配，索引会失效
- 用or分割开的条件，如果or前的条件列有索引，而后面的列没有索引，那么涉及的索引都不会被用到
- 如果MYSQL评估使用索引比全表更慢，则不适用索引



#### SQL提示

在SQL语句中假如一些人为的提示来达到优化操作的目的

```sql
select * from tb use index(id_index)where id=1;//建议mysql使用id_index这个索引，实际是否使用由mysql决定
select * from tb ignore index(id_index)where id=1;//让mysql忽略id_index这个索引
select * from tb force index(id_index)where id=1;//强制要求mysql使用id_index这个索引
```



#### 覆盖索引

尽量查询需要的列能在索引中全部找到，减少select*,因为这样不需要再用聚集索引去再查一遍数据



#### 前缀索引

当字段类型为字符串时(varchar,text)等，有时候需要索引很长的字符串，这会让索引变得很大，查询时，浪费大量的磁盘IO，影响查询效率，此时可以只将字符串的一部分前缀建立索引，这样可以大大节约空间，从而提高索引效率

```sql
create index idx_xxxx on table_name(column(n));
```

 **前缀长度**

可以根据索引的选择性来决定，选择性=不重复索引值/数据表的记录总数，索引选择性越高则查询效率越高

```sql
select count(distinct substring(email,1,5))/count(*) from tb;
```



#### 单列索引和联合索引

- 单列索引：即一个索引只包含单个列
- 联合索引：即一个索引包含多个列

在业务场景中，如果存在多个查询条件，考虑建立联合索引

多条件查询时，MYSQL优化器会评估那个字段索引效率更高，选择使用该索引完成本次查询（只会选择一个索引来查询）



### 索引设计原则

- 针对常作为查询条件(where),排序(order by),分组(group by)操作的字段建立索引
- 尽量选择区分度高的列建立索引，尽量建立唯一索引，区分度越高，使用索引的效率越高
- 如果是字符串类型的字段并且字段长度较长，可以建立前缀索引
- 尽量使用联合索引，减少单列索引，查询时，联合索引很多时候可以覆盖索引，节省存储空间，避免回表，提高查询效率
- 控制索引的数量，索引越多，维护索引结构的代价也就越大，会影响增删改的效率
- 如果存储列不能存储NULL值，在创建表的时候用NOT NULL约束它，当优化器知道每列是否包含NULL值时，它可以更好的确定哪个索引最有效的用于查询



## SQL优化

### 插入数据

#### insert优化

- 批量插入，一条insert语句多插入几条数据

  ```sql
  insert into tb values(1,'Tom'),(2,'Cat'),(3,'Jerry');
  ```

- 手动提交事务

  ```sql
  start transaction;
  insert into tb values(1,'Tom'),(2,'Cat'),(3,'Jerry');
  insert into tb values(1,'Tom'),(2,'Cat'),(3,'Jerry');
  insert into tb values(1,'Tom'),(2,'Cat'),(3,'Jerry');
  commit;
  ```

- 主键尽量按照顺序插入



#### 大批量插入数据

如果一次性需要插入大批量数据，使用insert语句插入性能较低，此时可以使用MYSQL数据库提供的load指令进行插入

```sql
#客户端连接服务器时，加上参数--local-infile
mysql --local-infile -u root -p
#设置全局参数为1，开启从本地加载文件导入数据的开关
set global local_infile =1;
#执行load指令将准备好的数据加载到表结构中
load data local infile '文件路径' into table 表名 fields terminated by ',' lines terminated by '\n';
```



### 主键优化

- 满足业务需求的情况下，尽量降低主键的长度
- 插入数据时，尽量选择顺序插入，选择使用auto_increment自增主键
- 尽量不要使用UUID或者其他自然主键，如身份证号
- 业务操作时，避免对主键的修改



### order by 优化

- Using filesort：通过表的索引或全表扫描，读取满足条件的数据行，然后在排序缓冲区sort buffer中完成排序操作，所有不是通过索引直接返回排序结果的排序都叫FileSort排序
- Using index：通过有序索引顺序扫描直接返回有序数据，这种情况即为using index,不需要额外排序，操作效率高

如果需要排序的内容满足一个索引的最左前缀，那么排序的时候就会使用这个索引

假如创建的索引两个字段都是升序的，如果排序的时候两个字段都升序或者两个字段都降序，那么会使用索引，如果一个升序一个降序那么不会使用这个索引，但可以在创建索引的时候指定一个升序一个降序

```sql
create index id_tb_age_phone on tb(age asc,phone desc);
```

如果不可避免的出现filesort,大数据量排序时，可以适当增大排序缓冲区大小sort_buffer_size(默认256k);



### group by 优化

同order by 优化



### limit 优化

无论加没加order by，使用limit都会对数据排序，比如limit 200000,10 需要排序前200010条数据，仅仅返回200000~200010的记录

**优化：**使用覆盖索引和子查询



### count优化

假如执行select count(*) from tb_user;

- MyISAM引擎把一个表的总行数存在磁盘上，效率很高
- InnoDB执行count(*)的时候，需要吧数据一行一行的从引擎里面读出来，然后累计计数

count()是一种聚合函数，对于返回的结果集，一行行的判断，如果count函数的参数不是NULL，累计值就加1，否则不加，最后返回累计值

#### count的几种用法

- count(主键)：行数
- count(字段)：统计该字段不为null的行数
- count(*)：行数
- **count(*)效率最高**



### update优化

update的条件如果没有索引，那么MYSQL会进行全表扫描导致表锁



## 视图/存储过程/触发器

### 视图

视图是一种虚拟存在的表，视图中的数据并不在数据库中实际存在，行和列数据来自自定义视图的查询中使用的表，并且是在使用视图时动态生成的

视图可以认为是对基表的一个引用

**创建**

```sql
create [or replace] view 视图名称[(列名列表)] as select [with[cascaded |local]check option]
e.g
create or replace view stu_v_1 as select id,name from student where id <=10;
```

**查询**

```sql
show create view 视图名称;#查看创建视图语句
select * from 视图名称...;查看视图数据
```

**修改**

```sql
create or replace view 视图名称[(列名列表)] as select语句 [with[cascaded |local]check option];
alter view 视图名称 as select语句 [with[cascaded |local] check option]
```

**删除**

```sql
drop view if exists 视图名称 
```

**对数据增删改的语法和select一样**，但在视图上对数据增删改本质上还是还是在基表上修改



#### 检查选项

当使用`with check option`子句创建视图时，如果此时在视图上插入修改删除数据，MYSQL会检查你操作的数据是否符合你建视图时的where语句，如果不符合会报错

```sql
create or replace view stu_v as select id,name from student where id<10 with check option;
insert into stu_v values(11,'11');//此时id=11>10
```

MYSQL支持基于另外一个视图创建视图，并且提供了两种选项

- cascaded（默认）：如果通过该视图修改数据，那么会给所有父视图包括自身都加上check option并检查所有父视图和自身的条件是否满足
- local：只会给当前视图加上check option 但是父视图本身就有check option 也会检查

```sql
create or replace view stu_v_1 as select id,name from student where id<20 ;
insert into stu_v_1 values(5,'111');
insert into stu_v_1 values(25,'111');#都不会报错因为没有加check option
create or replace view stu_v_2 as select id,name from stu_v_1 where id >10 with cascaded check option;
insert into stu_v_2 values(5,'111');#报错不满足>10
insert into stu_v_2 values(25,'111');#报错不满足<20
insert into stu_v_2 values(15,'111');#满足
create or replace view stu_v_3 as select id,name from stu_v_1 where id <25 with local check option;
insert into stu_v_3 values(5,'111');#报错不满足stu_v_2 >10
insert into stu_v_3 values(30,'111');#报错不满足<25
insert into stu_v_3 values(23,'111');#满足,因为不会检查stu_v_1
```



#### 更新

要使视图可更新，视图中的行和基础表中的行必须存在一对一的关系，如果视图中包含下面任何一项，那么该视图不可更新

- 聚合函数或窗口函数（sum(),min(),max(),count()等）
- distinct
- group by
- having
- union或者union by



#### 作用

- 简单：视图不仅可以简化用户对数据的理解，也可以简化他们的操作，那么被经常使用的查询可以被定义为视图，从而使得用户不必为后面的操作每次指定全部的条件
- 安全：数据库可以授权，但是不可以授权到数据库特定的行和特定的列上，通过视图用户只能查询和修改他们能看得到的数据
- 数据独立：视图可帮助用户屏蔽真实表结构变化带来的影响





### 存储过程

存储过程是事先经过编译并存储在数据库的一段SQL语句集合，就是数据库SQl语言层面代码的封装和重用

**特点**

- 封装，复用
- 可以接收参数，也可以返回数据
- 减少网络交互，效率提高

```sql
//创建存储过程
create procedure 存储过程名称([参数列表])
begin
    --SQL语句
end;
//调用存储过程
call 名称([参数]);
//查看存储过程
select * from information_schema.routines where routine_schema='数据库名';#查询指定数据库的存储过程及其状态信息
show create procedure 存储过程名称;#查询某个存储过程的定义
//删除存储过程
drop procedure [if exists]存储过程名称;
```

**ps:在命令行中，执行创建存储过程的SQL时，需要通过关键词delimiter指定SQL语句的结束符**

```sql
delimiter $$
create procedure p1()
begin
    select * from student;
end $$
```



#### 变量

##### 系统变量

是MYSQL服务器提供的，不是用户定义的，属于服务器层面，分为全局变量(global)，会话变量(session)

```sql
show [session|global] variables;#查看所有系统的变量
show [session|global] variables like '..';#可以通过模糊匹配方式查找变量
select @@[session|global]系统变量名;#查看指定变量的值
select @@autocommit;或者 select @@session.autocommit;
set [session|global] 系统变量名=值;
set @@[session|global] 系统变量名=值;
```

**ps:**

- 如果没有指定session/global 默认是session
- mysql服务重新启动之后，所设置全局参数会失效，要想不失效，可以在/etc/my.cnf中配置



##### 用户自定义变量

是用户根据自己定义的变量，用户变量不用提前声明，在用的时候直接用“@变量名”使用就可以，其作用域为当前连接

```sql
#赋值
set @var_name=expr[,@var_name=expr]...;
set @var_name:=expr[,@var_name:=expr]...;
select @var_name:=expr[,@var_name:=expr]...;
select 字段名 into @var_name from 表名;
#使用
select @var_name;
//e.g
set @myname :='creep';
set @myname :='creep',@myage:=19;//一次性赋值多个
select count(*) into @my_count from tb;//将字段赋值给变量
select @myname//查看变量
```



##### 局部变量

是根据需要定义的在局部生效的变量，访问之前，需要declare声明，可用作存储过程内的局部变量和输入参数，局部变量的范围是在其内声明的begin...end块

```sql
#声明
declare 变量名 变量类型[default 默认值];
变量类型就是数据库的基本类型 int,bigint,char,varchar等
#赋值
set 变量名=值;
set 变量名:=值;
select 字段名 into 变量名 from 表名;
#e.g
create procedure p2()
begin
    declare stu_count int default 0;
    select count(*) into stu_count from student;
    select stu_count;
end;
```



#### if判断

```sql
if 条件1 then
	...
elseif 条件2 then   --可选
	...
else 			   --可选
	...
end if;
 
```



#### 参数(in,out,inout)

**类型**

- **in**：该类参数作为输入，需要调用时传入值
- **out**：该类参数作为输出，也就是该参数可以作为返回值
- **inout**：既可以作为输入参数，也可以作为输出参数

**用法**

```sql
create procedure 存储过程名称(in/out/inout 参数名 参数类型)
begin

end;
#e.g
create procedure p2(in score int,out grade varchar(10))
begin
    if score >=85 then
        set grade :='优秀';
    elseif score >=60 then
        set grade :='及格';
    else
        set grade :='不及格';
    end if;
end;
call p2(58,@result);
select @result;
```



#### case

```sql
#语法1
case case_value
    when when_value1 then statement_list1 #如果case_value==when_value1
    when when_value2 then statement_list2...
    else statement_list
end case;
#语法2
case
    when search_condition1 then statement_list1 #如果search_condition1为true
    when search_condition2 then statement_list2
    else statement_list
end case;
```



#### while

```sql
#先判定条件，true执行，否则不执行
while 条件 do
   sql逻辑
end while;
```



#### repeat

```sql
#会先执行一次逻辑，然后判断逻辑是否满足，如果满足则退出否则继续执行
repeat 
    sql逻辑;
    until 条件
end repeat;
```



#### loop

loop实现简单的循环，如果不在SQL逻辑中增加退出循环的条件，可以用其来实现简单的死循环，loop可以配合下面两个语句使用

leave：配合循环使用他，退出循环

iterate：必须用在循环中，作用是跳过当前循环剩下的语句，直接进入下一次循环

```sql
[begin_label:] loop
	sql 逻辑
end loop [end_label];
leave label;退出指定的循环体
iterate label;直接进入下一次循环
#e.g
create procedure p3(in n int)
begin
    declare an int default 0;
    sum:loop
    if n<=0 then
        leave sum;
    end if;
    set an:=an+n;
    set n:=n-1;
    end loop sum;
    select an;
end;
```





#### 游标cursor

游标是用来存储查询结果集的数据类型，在存储过程和函数中可以使用游标对结果集进行循环的处理

```sql
#声明游标
declare 游标名称 cursor for 查询语句：
#打开游标
open 游标;
#获取游标记录
fetch 游标名称 into 变量[,变量];
#关闭游标
close 游标名称;

#e.g
create procedure p3(in _age int )
begin
    declare _name varchar(10);
    declare  _profession varchar(10);
    declare cur cursor for select name,profession from student where age <=_age;#普通变量必须声明在游标前面
    create table if not exists stu(
      id int primary key auto_increment,
      name varchar(10),
      profession varchar(10)
    );
    open cur;
    while true do
        fetch cur into _name,_profession;
        insert into stu values(null,_name,_profession);
        end while;
    close cur;
end;
```



#### 条件处理程序-handler

上面的程序其实会出现死循环，因为并不知道游标什么时候读完会出现死循环

条件处理程序可以用来定义在流程控制结构执行过程中遇到问题时相应的处理步骤

```sql
declare handler_action handler for condition_value[,conditon_value]...statement;
handler_action:
    continue:继续执行当前程序
    exit:终止执行当前程序
condition_value:
    sqlstate sqlstate_value:状态码 如02000
    sqlwarning 所有以01开头的sqlstate代码的简写
    not found 所有以02开头的sqlstate代码的简写
    sqlexception 所有没有被sqlwarning或not found捕获的sqlstate代码的简写
    
#例如上面代码可以加上这样一句话
declare exit handler for sqlstate '02000' close cur;
```



### 存储函数

存储函数是由返回值的存储过程，存储函数的参数只能是in

```sql
create function 存储函数名称([参数列表])
returns type [characteristic...]
begin
    --sql语句
    return ...;
end;

characteristic 说明
- deterministic :相同的输入参数总是产生相同的结果
- no sql :不包含sql语句
- reads sql data :包含读取数据的语句，但不包含写入数据的语句


#e.g

create function f2(n int)
returns int deterministic
begin
    declare an int default 0;
    while n>0 do
        set an:=an+n;
        set n:=n-1;
        end while;
    return an;
end;
select f2(10);
```





### 触发器

触发器是与表有关的数据库对象，在insert/update/delete之前或者之后，触发并执行触发器中定义的SQL语句集合，触发器的这种特性可以协助应用在数据库段确保数据的完整性，日志记录，数据校验等操作

使用别名old和new来引用触发器中发生变化的记录内容，这与其他的数据库是相似的，现在触发器还支支持行级触发，不支持语句级触发

- insert型触发器 ：new表示将要或者已经新增的数据
- update型触发器：old表示修改之前的数据，new表示将要修改或者已经修改后的数据
- delete型触发器：old表示将要或者已经删除的数据

```sql
#创建
create trigger trigger_name
before/after insert/update/delete
on tb_name for each row #行级触发器
begin
    sql语句
end;
#查看
show triggers;
#删除
drop trigger [schema_name]trigger_name;#如果没有指定数据库，默认为当前数据库

#通过触发器记录user表的数据变更日志
create trigger tb_user_insert_trigger
    after insert
    on tb_user for each row
begin
    insert into user_logs values(now(),concat('id=',new.id,'name',new.name));
    #这里的new代指插入tb_user的那行数据
end;
```



## 锁

### 全局锁

全局锁就是对整个数据库实例加锁，加锁后整个实例就处于只读状态，后续的dml写语句ddl语句，语句更新操作的是事务提交语句都将被阻塞

典型的应用场景是做全库的逻辑备份，对所有的表进行锁定，从而获取一致性视图，保证数据的完整性

```sql
flush tables with read lock;#将当前数据库的所有表上锁
 mysqldump -uroot -p060622 数据库名 > ~/creep.sql#将整个数据库备份
unlock tables;#解锁，未解锁之前只能select 不能修改删除表数据
```

**特点**

数据库中加全局锁，是一个比较重的操作，存在以下问题：

- 如果在主库上备份，那么在备份期间都不能执行更新，业务基本都要停摆
- 如果在从库上备份，那么在备份期间从库不能执行主库同步过来的二进制日志，会导致主从延迟

在InnoDB引擎中，可以在备份时加上参数--single-transaction 参数来完成不加锁的一致性数据备份



### 表级锁

每次操作锁住整张表，锁定粒度大，发生锁冲突的概率最高，并发度低，应用在MYlSAM，InnoDB，BDB等存储引擎中



#### 表锁

- 表共享读锁（read lock），如果当前窗口加了read 锁，当前窗口可以使用DQL语句，DDL/DML语句会报错，其他窗口可以使用DQL语句，DDL/DML语句会阻塞直到释放锁
- 表独占写锁（write lock），如果当前窗口加了write锁，当前窗口可以使用DQL和DDL/DML语句，其他窗口DQL和DDL/DML语句会阻塞

```sql
lock tables 表名[,表名2...] read/write #加锁
unlock tables;/客户端断开连接 #释放锁
```



#### 元数据锁

**在表上有活动事务的时候，不可以对表结构进行修改**

MDL加锁过程是系统自动控制，无需显式使用，在访问一张表的时候会自动加上

当对一张表进行增删改查的时候，加MDL读锁(共享)，当对表结构机械能变更操作的时候，加MDL写锁(排他)

```sql
#查看元数据锁
select object_type,object_schema,object_name,lock_type,lock_duration from performance_schema.metadata_locks;
```



#### 意向锁

为了避免DMl在执行时，加的行锁与表锁的冲突，在InnoDB中引入了意向锁，使得表锁不用检查每行数据是否加锁，使得意向锁来减少表锁的检查

- 意向共享锁（IS）:由语句 select ...lock in share mode 添加，与表锁read兼容，与表锁write互斥
- 意向排他锁（IX）:由insert ，update，delete，select...for update 添加，与表锁read和write都互斥
- 意向锁之间不会互斥

```sql
#查看意向锁及行锁的加锁情况
select object_schema,object_name,index_name,lock_type,lock_mode,lock_data from performance_schema.data_locks;
```



### 行级锁

每次操作锁住对应的行数据，锁定粒度最小，发生锁冲突的概率最低，并发度最高，应用在InnoDB存储引擎中

- 行锁：锁定单个行记录的锁，防止其他事务对此行进行update和delete,在RC,RR隔离级别都支持
- 间隔锁：锁定索引记录的间隙(不含该记录)，确保所有记录间隙不变，防止其他事务在这个间隙进行insert，产生幻读，在RR隔离级别下都支持
- 临键锁：行锁和间隙锁组合，同时锁住数据，并锁住数据前面的间隙Gap，在RR隔离级别下支持



#### 行锁

InnoDB实现了以下两种类型的行锁

- 共享锁(S)：允许一个事务去读一行，阻止其他事务获得相同数据集的排他锁
- 排他锁(X)：允许获取排他锁的事务更新数据，阻止其他事务获得相同数据集的共享锁和排他锁

| 当前锁类型\请求锁类型 | S(共享锁) | X(排他锁) |
| --------------------- | --------- | --------- |
| S(共享锁)             | 兼容      | 冲突      |
| X(排他锁)             | 冲突      | 冲突      |



| SQL                         | 行锁类型   | 说明                                     |
| --------------------------- | ---------- | ---------------------------------------- |
| insert...                   | 排他锁     | 自动加锁                                 |
| update...                   | 排他锁     | 自动加锁                                 |
| delete...                   | 排他锁     | 自动加锁                                 |
| select(正常)                | 不加任何锁 |                                          |
| select...lock in share mode | 共享锁     | 需要手动在select之后加lock in share mode |
| select ...for update        | 排他锁     | 需要手动在select之后加for update         |



默认情况下，InnoDB在repeatable read事务隔离级别运行，InnoDB使用next-key锁进行搜索和索引扫描，以防止幻读

- 针对唯一索引进行检索的时候，对已存在的记录进行等值匹配时，将会自动优化为行锁
- InnoDB的行锁是针对索引加的锁，不通过索引条件检索数据，那么InnoDB将对表中的所有记录加锁，此时就会升级为表锁





#### 间隙锁/临键锁

默认情况下，InnoDB在repeatable read 事务隔离级别运行，InnoDB使用next-key 锁进行搜索和索引扫描，以防止幻读

- 索引上的等值查询（唯一索引），给不存在的记录加锁时，优化为间隙锁
- 索引上的等值查询（普通索引），向右遍历时最后一个值不满足查询要求时，next-key lock退化为间隙锁
- 索引上的范围查询（唯一索引）--会访问到不满足条件的第一个值为止

**间隙锁唯一目的时防止其他事务插入间隙，间隙锁可以共存，一个事务采用的间隙锁不会阻止另一个事务在同一个间隙上采用间隙锁**





## InnoDB引擎

### 逻辑存储结构

- 表空间（ibd文件），一个mysql实例可以对应多个表空间，用于存储记录，索引等数据
- 段，分为数据段，索引段，回滚段，InnoDB时索引组织表，数据段就是B+树的非叶子节点，索引段就是B+树的非叶子节点，段用来管理多个区
- 区，表空间的单元结构，每个区的大小为1M，默认情况下，InnoDB存储引擎页大小为16K，即一个区中一共有64个连续的页
- 页，是InnoDB存储引擎磁盘管理的最小单元，每个页的大小默认是16kb，为了保证页的连续性，InnoDB存储引擎每次从磁盘申请4-5个区
- 行，InnoDB存储引擎数据是按行进行存放的



### MVCC

#### 基本概念

**当前读**

读取的是记录的最新版本，读取时还要保证其他并发事务不能修改当前记录，会对读取的记录进行加锁，对于我们日常的操作，如：select ... lock in share mode(共享锁)，select ... for update，update，insert，delete(排他锁)都是一种当前读

**快照读**

简单的select(不加锁)就是快照读，快照都，读取的是记录的可见版本，有可能是历史数据，不加锁，是非阻塞读

- read committed :每次select,都生成一个快照读
- repeatable read:开启事务后第一个select语句才是快照读的地方
- serializable：快照都会退化为当前读

**MVCC**

全称Multi-Version Concurrency Control,多版本并发控制，指维护一个数据的多个版本，使得读写操作没有冲突，快照读为MYSQL实现MVCC提供了一个非阻塞读功能，MVCC的具体实现，还需要依赖数据库记录中的三个隐式字段，undo log日志，readView





#### **隐藏字段**

- DB_TRX_ID :最近修改事务ID，记录插入这条记录或最后一条修改该记录的事务ID
- DB_ROLL_PTR: 回滚指针，指向这条记录的上一个版本，用于配合undo log，指向上一个版本
- DB_ROW_ID :隐藏主键，如果表结构没有指定主键，将会生成该隐藏字段



#### **undo log**

回滚日志，在insert，update，delete的时候产生的便于数据回滚的日志

在insert的时候产生的undo log日志只在回滚的时候需要，在事务提交后，可被立即删除

而update，delete的时候，产生的undo log日志不仅在回滚的时候需要，在快照读的时候也需要，不会被立即删除



#### **readview**

Readview（读视图）是快照读SQL执行时MVCC提取数据的依据，记录并维护系统当前活跃事务（未提交的）ID

ReadView中包含了四个核心字段

| 字段           | 含义                                                 |
| -------------- | ---------------------------------------------------- |
| m_ids          | 当前活跃的事务ID集合                                 |
| min_trx_id     | 最小活跃事务ID                                       |
| max_trx_id     | 预分配事务ID，当前最大事务ID+1（因为事务ID是自增的） |
| creator_trx_id | ReadView创建者的事务ID                               |



**版本链数据访问规则**

`trx_id`：代表是当前事务id

- trx_id==creator_trx_id 可以访问该版本 说明数据是在这个事务更改的
- trx_id<min_trx_id 可以访问该版本 说明数据已经提交了
- trx_id >max_trx_id 不可以访问该版本 说明事务是在ReadView生成后才开启
- min_trx_id<=trx_id<=max_trx_id 如果trx_id 不在m_ids中可以访问该版本 说明数据已经提交



**不同的隔离级别，生成ReadView的时机不同**

- Read Commited：在事务中每执行一次快照读生成ReadView
- Repeatable Read：仅在事务中第一次执行快照读时生成ReadView ，后续复用该ReadView







## MYSQL管理

### 系统数据库

MYSQL自带四个数据库

| 数据库             | 含义                                                         |
| ------------------ | ------------------------------------------------------------ |
| mysql              | 存储MYSQL服务器正常运行所需要的各种信息（时区，主从，用户，权限） |
| information_schema | 提供了访问数据库元数据的各种表和视图，包含数据库，表，字段类型及访问权限 |
| performance_shema  | 为MYSQL服务器运行时状态提供了一个底层监控功能，主要用于收集数据库服务器性能参数 |
| sys                | 包含了一系列方便DBA和开发人员利用performance_schema 性能数据库进行性能调优和诊断的视图 |



### 常用工具

#### mysql

该mysql不是指mysql服务，而是指mysql客户端工具

```sql
语法：
	mysql [options] [database]
选项:
	-u,--user=name #指定用户名
	-p,--password[=name] #指定密码
	-h,-host=name #指定服务器IP或域名
	-P,-port=port #指定连接端口
	-e,--execute=name #执行SQL语句并退出
-e选项可以在Mysql客户端执行SQL语句，而不用连接到MYSQL数据库再执行，对于一些批处理脚本，这种方式尤其方便
e.g
mysql -uroot -p123456 数据库名 -e"select * from stu";
```



#### mysqladmin

mysqladmin 是一个执行管理操作的客户端程序，可以用它来检查服务器的配置和当前状态，创建并删除数据库等

```sql
通过帮助文档查看选项
	mysqladmin --help
e.g
	mysqladmin -uroot -p123456 drop 'test01';#删除数据库test01
	mysqladmin -uroot -p123456 version;#查看mysql版本信息
```



#### mysqlbinlog

由于服务器生成的二进制日志文件以二进制格式保存，所以如果想要检查这些文本的文本格式，就会使用到mysqlbinlog日志管理工具

```sql
语法：
	mysqlbinlog [options] log-file1 log-file2...
选项：
	-d，--database=name 指定数据库名称，只列出指定的数据库相关操作
	-o，-offset=# 忽略掉日志中的前n行命令
	-r,-result-file=name 将输出的文本格式日志输出到指定文件
	-s,--short-form 显示简单格式，省略掉一些信息
	--start-datatime=date1 --stop-datetime=date2 指定日期间隔内的所有日志
	--start-position=pos1 --stop-position=pos2 指定位置间隔内的所有日志
```



#### **mysqlshow**

mysqlshow客户端对象查找工具，用来很快的查找存在哪些数据库，数据中的表，表中的列或者索引

```sql
语法：
	mysqlshow [options][db_name[table_name[col_name]]]
选项：
	-count 显示数据库及表的统计信息（数据库，表均可以不指定）
	-i 显示指定数据库或者指定表的状态信息
e.g
	#查询每个数据库表的数量及表中记录的数量
	mysqlshow -uroot -p密码 --count
	
	#查询test库中每个表中的字段数及行数
	mysqlshow -uroot -p密码 test --count
	
	#查询test库中book表的详细情况
	mysqlshow -uroot -p密码 test book --count
```



#### mysqldump

mysqldump客户端工具用来备份数据库或在不同数据库之间进行数据迁移，备份内容包含创建表，插入表的sql语句

```sql
语法：
	mysqldump[options]do_name[tables]
	mysqldump[options]--database/-B db1 [db2 db3...]
	mysqldump[options]--all-databases/-A
连接选项：
	-u --user=name 指定用户名
	-p --password[=name] 指定密码
	-h --host=name 指定服务器ip或域名
	-P --port=# 指定连接端口
输出选项：
	--add-drop-database 在每个数据库创建语句前加上drop database语句
	--add-drop-table 在每个表创建语句前加上drop table语句，默认开启；不开启（--skip-add-drop-table)
	-n,--no-create-db 不包含数据库的创建语句
	-t,--no-create-info 不包含数据表的创建语句
	-d，--no-data 不包含数据
	-T，--tab=name 自动生成两个文件：一个.sql文件，创建表结构的语句：一个.txt文件，数据文件
	#如果要使用-T选项生成的.sql和.txt文件只能放在mysql信任的目录下，通过环境变量查看show variables like 				'%secure_file_priv%'
	
e.g
mysqldump -uroot -p060622 -t creep>1.sql #.sql中只有插入数据的语句没有创建表的语句
mysqldump -uroot -p060622 -T /var/lib/mysql-files/ creep tb
```



#### mysqlimport/source

mysqlimport 是客户端数据导入工具，用来导入mysqldump 加-T参数后导入的文本文件

```sql
语法：
	mysqlimport[options] db_name textfile1 [testfile2..]
示例：
	mysqlimport -u root -p060622 creep /var/lib/mysql-files/tb.txt
```

如果需要导入sql文件，可以使用mysql中的source指令

```sql
source /root/xxxx.sql
```





# 运维

## 日志

### 错误日志

错误日志是MYSQL中最重要的日志之一，它记录了当MYSQL启动和停止时，以及服务器在运行过程中发生任何严重的错误时的相关信息。当数据库出现任何故障导致无法正常使用时，建议首先查看此日志，该日志默认是开启的

```sql
show variables like '%log_error%' #查看日志的位置
```





### 二进制日志

二进制日志（binlog）记录了所有的DDL（数据定义语言）语句和DML（数据操纵语言）语句，但不包含数据查询(select,show)语句

**作用**：灾难时的数据恢复，MYSQL的主从复制

默认二进制日志开启着

```sql
show variables like '%log_bin%' #涉及到的参数
```



#### 日志格式

MYSQL服务器提供了多种格式来记录二进制日志

| 日志格式  | 含义                                                         |
| --------- | ------------------------------------------------------------ |
| statement | 基于SQL语句的日志记录，记录的是SQL语句，对数据进行修改的SQL都会记录在日志文件中 |
| row       | 基于行的日志记录，记录的是每一行的数据变更（默认）           |
| mixed     | 混合了statement和row两种格式，默认采用statement，在某些特殊情况会自动切换为row进行记录 |

```sql
show variables like '%binlog_format%';#查看日志格式
```



#### 日志查看

由于日志是以二进制形式存储的，不能直接读取，需要通过二进制日志查询工具mysqlbinlog来查看

```sql
mysqlbinlog [参数选项] logfilename
参数选项：
	-d 指定数据库的名称
	-o 忽略掉日志中的前n行命令
	-v 将行事件（数据变更）重构为sql语句，如果日志格式为row查看的时候要加这个选项
	-vv 将行事件（数据变更）重构为sql语句，并输出注释信息
```



#### 日志删除

对于比较繁忙的业务系统，每天生成的binlog数据巨大，如果长时间不清除，将会占用大量磁盘空间，可以通过下面方式清理日志

| 指令                                             | 含义                                                         |
| ------------------------------------------------ | ------------------------------------------------------------ |
| reset master                                     | 删除全部的binlog日志，删除之后，日志编号将从binlog.000001重新开始 |
| purge master logs to `'binlog.******'`           | 删除`******`编号之前的所有日志                               |
| purge master logs before 'yyyy-mm-dd hh24:mi;ss' | 删除日志为'yyyy-mm-dd hh24:mi;ss'之前产生的所有日志          |

也可以在mysql配置文件中配置二进制文件日志的过期时间，设置了之后，二进制日志过期会自动删除





### 查询日志

查询日志中记录了客户端的所有操作语句，而二进制日志不包含查询数据的SQL语句，默认情况下，查询日志是未开启的

```sql
show variables like'%general%';
```





### 慢查询日志

慢查询日志记录了所有执行时间超过参数long_query_time 设置值并且扫描记录数不小于min_examined_row_limit 的所有的SQL语句的日志，默认未开启，long_query_time默认为10s，最小为0，精度可到微秒

```sql
#慢查询日志
slow_query_log=1
#执行时间参数
long_query_time=2
```

默认情况下，不会记录管理语句，也不会记录不使用索引进行查找的查询，可以使用log_slow_admin_statements和更改此行为log_queries_not_using_indexes

```sql
#记录执行较慢的管理语句
log_slow_admin_statements=1
#记录执行较慢的未使用索引的语句
log_queries_not_using_indexes=1
```



## 主从复制

主从复制是指将主数据库的DDL和DML操作通过二进制日志传到从库数据库中，然后在从库上对这些日志重新执行（也叫重做），从而使得从库和主库的数据保证同步

MYSQL支持一台主库同时向多台从库进行复制，从库同时也可以作为其他从服务器的主库，实现链状复制

**优点：**

- 主库出现问题，可以快速切换到从库提供服务
- 实现读写分离，降低主库的访问压力
- 可以在从库中执行备份，以避免备份期间影响主库服务



**原理：**

- Master 主库在事务提交时，会把数据变更记录在二进制文件Binlog中
- 从库读取主库的二进制日志文件Binlog，写入到从库的中继日志Relay Log
- slave重做中继日志中的事件，将改变反映它自己的数据



### 搭建

#### **服务器准备**

```sql
开放指定的3306端口号
firewall-cmd --zone=public --add-port=3306/tcp -permanent
firewall-cmd -reload
```



#### **主库配置**

- 修改配置文件

  ```sql
  #mysql服务id，保证整个集群环境中唯一，取值范围为1~2^32-1，默认为1
  server-id=1
  #是否可读，1代表只读，0代表读写
  read-only=0
  #忽略的数据，之不需要同步的数据库
  #binlog-ignore-db=mysql
  #指定同步的数据库
  #binlog-do-db=db01
  ```

- 重启mysql服务器

  ```sql
  systemctl restart mysqld
  ```

- 登录mysql，创建远程连接的账号，并授予主从复制权限

  ```sql
  #创建creep用户，并设置密码，该用户可在任意主机连接该MYSQL服务
  create user 'creep'@'%' identified with mysql_native_password by 'Root@123456'
  #为creep用户分配主从复制权限 
  grant replication slave on *.* to 'creep'@'%';
  ```

- 通过指令，查看二进制日志文件

  ```sql
  show master status;
  ```



#### 从库配置

- 修改配置文件

  ```sql
  #mysql服务id，保证整个集群环境中唯一，和主库不一样即可
  server-id=2
  read-only=1
  ```

- 重启mysql服务

  ```sql
  systemctl restart mysqld
  ```

- 登录mysql，设置主库配置

  ```sql
  change replication source to source_host ='xxx.xxx',source_user='xxx',source_password='xxx',source_log_file='xxx',source_log_pos=xxx;
  #如果mysql是8.0.23之前的版本将上面的source换成master
  host:ip地址
  user:连接主库的用户名
  password:连接主库的密码
  log_file:binlog日志文件名
  log_pos:binlog日志文件位置
  ```

- 开启同步操作

  ```sql
  start replica;#8.0.22之后
  start slave;#8.0.22之前
  ```

- 查看主从同步状态

  ```sql
  show replica status;#8.0.22之后
  show slave status;#8.0.22之前
  ```







## 分库分表



分库分表的中心思想都是将数据分散存储，使得单一数据库/表的数据量变小来缓解单一数据库的性能问题，从而达到提升数据库性能的目的





​	
