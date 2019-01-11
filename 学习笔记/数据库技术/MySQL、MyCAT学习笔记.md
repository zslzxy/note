[TOC]
# MySQL学习笔记

### 一、centos7安装MySQL5.6版本：
**网上安装教程**：https://www.cnblogs.com/bigbrotherer/p/7241845.html

第一步：wget -i -c http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm  
第二步：yum -y install mysql57-community-release-el7-10.noarch.rpm  
第三步：yum -y install mysql-community-server  
第四步：启动mysql  systemctl start  mysqld.service  
第五步：查看mysql状态 systemctl status mysqld.service  
第六步：获取初始密码 grep "password" /var/log/mysqld.log  
第七步：进入数据库库  mysql -uroot -p  
第八步：修改密码 ALTER USER 'root'@'localhost' IDENTIFIED BY ‘123456’;   
如果报错，就执行  

```
set global validate_password_policy=0;
set global validate_password_length=1;
```

第九步：设置开机自动启动   systemctl enable mysqld  
第十步：设置字符集 ：
修改/etc/my.cnf配置文件，在[mysqld]下添加编码配置，如下所示：

```
[mysqld]
character_set_server=utf8
init_connect='SET NAMES utf8'
```

第十一步：查看mysql安装位置  
![image](https://note.youdao.com/yws/api/personal/file/AC5496E4DC1F4F64A7ED80813D4AE5B8?method=download&shareKey=e2b3ba4f20fbaa4917a46a1047a5fe2a)


### 二、centos7安装MySQL5.7版本

1、搭建仓库:  wget https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm  
2、安装好仓库:  rpm -ivh mysql57-community-release-el7-11.noarch.rpm  
3、安装mysql:   yum -y install mysql-community-server.x86_64  
4、启动mysql以后：获取初始密码 grep "password" /var/log/mysqld.log，登录后。  

```
set global validate_password_policy=0;
set global validate_password_length=1;
set password = password("新密码");
```


5、设置字符集 ：
修改/etc/my.cnf配置文件，在[mysqld]下添加编码配置，如下所示：

```
[mysqld]
character_set_server=utf8
init_connect='SET NAMES utf8'
```

6、查看mysql的字符集编码：show variables like 'character%';

**注意：**
mysql忘记密码：
   启动好mysql服务器后，在系统根目录输入: 	mysql_secure_installation 重新设置密码。


### 三、SQL语言
#### SQL（Structred Query Language --- 结构化查询语言）。  

SQL语言最主要的作用是用于数据存取、查询数据、更新数据、管理关系型数据系统，主要分为四种类型：  
1. DDL语句：数据库定义语句---数据表(表的创建、修改、删除等操作)、库、视图、索引、存储过程的操作。
2. DML语句：数据库操纵语言---数据insert、update、delete语句。
3. DCL语句：数据库控制语言---用户的权限等操作(GRANT、REVOKE)。
4. DQL语句：select语句。

#### 系统数据库：
1）information_schema：虚拟局，存储了系统中一些数据库对象的信息  
2）performance_schema： 主要存储数据库的性能参数  
3）mysql： 授权库，主要存储系统用户的权限信息  
4）sys： 主要存储数据库服务器的性能参数  


### 四、MySQL数据类型
##### 1、int 类型
可以存长度为 10 位的数字。可以使用unsigned修饰，用于表示无符号。  

**注意：**
1）使用 unsigned  关键字来修饰 int 数据类型，只能输入正数。   
2）一般 int 类型不显示的限制长度，应为即是长度超出了也能够存储(总体长度不超过10位)。

##### 2、float 类型：浮点数类型
创建：create table t1(money float(5,2));   //指定的数据的数字只能有5位，小数点占两位  

##### 3、char、varchar字段
- char : 自定义长度
- varchar : 定义长度以后，根据插入的数据的长度来自动分配空间

##### 4、date、time、datetime、timestamp字段：
- date : 年.月.日
- time : 时.分.秒
- datetime : 年.月.日.时.分.秒
- timestamp : 当前时间(等价 datetime )  //如果未给这个位置赋值，则自动填入当前时间
- year : 年

##### 5、枚举类型enum
- enum  单选---只能在给定的范围内选择一个值。
- set    多选---在给定范围内选择一个或者多个值。

使用方式：create table t6(name varchar(50),sex enum('m','f'),hobby set('book','disc','game');  

插入数据：insert into t6 values('zsl','m','game,book');      //只能在给定范围设置值


### 五、Mysql表操作 DDL
#### 1、创建表

```sql
create table tt(.....);
```
#### 2、修改表

##### 1）修改表名：
alert table 表名 rename 新表名;


##### 2）增加字段：
1）alter table 表名 add 字段名 数据类型 [完整性约束条件];

2）在第一个位置添加字段: alter table 表名 add 字段名 数据类型 [完整性约束条件] first;

3）在指定位置添加字段：alter table 表名 add 字段名 数据类型 [完整性约束条件] after字段名; 

##### 3）修改字段：
1）modify 字段名 数据类型 [完整性约束条件];

2）alter table 表名 change 旧字段名 新字段名 [完整性约束条件];


#### 3、删除表
##### 删除表或者表数据：
1）delete from 表名      //删除表中所有的数据 

2）delete from 表名 where 条件      //根据条件删除表中数据

3）drop table 表名     //删除表
##### 删除字段：
alter table 表名 drop 字段名;


#### 4、查看表结构
- DESCRIBE 查看表结构;

- DESCRIBE 表名; 

- DESC 表名; 

- SHOW CREATE TABLE 查看表详细结构;

- SHOW CREATE TABLE 表名;


#### 5、表完整性约束：
- PRIMARY KEY (PK) 表示该字段为该表的主键，可以唯一的标识记录，不可为空，不可重复。通常是单个主键，但是一张表可以设置多个字段为主键，这样的叫做复合组件。每个主键起唯一性的约束作用，且会创建主键索引。

- FOREIGN KEY (PK) 标识该字段为该表的外键，可以实现表之间的关联。规范数据的完整性，同时也在这个key上创建一个index。
- NOT NULL 表示该字段不能够为空。
- UNIQUE KEY (UK) 标识该字段的值是唯一的，可以为空，一个表可以有多个 UNIQUE KEY。规范的是数据的唯一性，并且会创建一个唯一索引。
- AUTO_INCREMENT 标识该字段的值自动增长(整数类型，自动增长)。
- DEFAULT 为该字段设置默认值。
- UNSIGNED 无符号，正数。
- ZEROFILL 使用0填充。

##### 例如：
```sql
例如：
create table stu1d(
    id int auto_increment,
    name varchar(30) unique,
    sex enum('m','f') not null default 'm',
    age int not null default 18,
    salary double not null,
    primary key(id)                       //默认的主键名字与这个字段名字相同
    foreign key(name) references person(name)    //将name字段作为外键指向person表的name
        on update cascade   //同步更新，当更新父表的外键的一些值，字表也会自动更新，可选
        on delete cascade    //同步删除，当删除父表的外键的一些值，子表也会自动删除，可选
);
```


### 六、数据的增删改(DML)与(DQL)
#### 1、DML操作
##### 1）插入(insert)
**完整性插入：**  
语法一：INSERT INTO 表名(字段 1,字段 2,字段 3…字段 n) VALUES (值 1,值 2,值 3… 值 n);  
语法二： INSERT INTO 表名 VALUES (值 1,值 2,值 3…值 n);  

**指定字段插入：**  
语法： INSERT INTO 表名(字段 2,字段 3…) VALUES (值 2,值 3…); 

**插入多条数据：**  
语法：insert into 表名 values(值 1,值 2),(值 1,值 2),(值 1,值 2);

**将查询到的结果插入到表中：**  
语法：insert into stud1   select * from stud2 where age=18;   //将查询到的数据插入到操作的表中。

##### 2）更新(update)
语法：update stud set 字段1=值1,字段2=值2 where id=12;

##### 3）删除(delete)
语法：delete from 表名 where 条件;

##### 4、简单查询(select)
1）最简单查询：  
select * from employee;

2）==避免重复(distinct)==：通常使用distinct来限制某一字段：  
select  distinct age from employee;

3）==四则运算查询==：  
select name,age,salary,salary+5 as more_money from employee;  
注意：salary+5 as more_money   表示为 salary+5 的那一列取别名。

4）==concat()函数==：用于连接字符串：  
select concat(name,salary) as conn from employee; 

**单条件查询：**  
1）最简单查询：  
select * from employee where name='zsl';

2）==多条件查询(使用 and 连接)==：  
select * from employee where post='hr' and salary=5000.0;

3）关键字查询：  
==between ：==  
select * from employee where salary between 5000 and 15000;  ---两者之间

==not between ：==  
select * from employee where salary not between 5000 and 15000;---两者之外

==is null ：==  
select * from employee where job is null;  --- job 字段为空

==is not null ：==  
select * from employee where job is not null;  ---job 字段不为空

==in(集合查询) ：==  
select * from employee where salary in(40,50,60); --查找工资为40、50、60的人

==like(模糊查询) ：==  
通配符：  
- % ---多个字符   
- _---表示单个字符   
- __---表示两个字符  
- ___---表示多个字符

==not like（模糊查询）:==  
例如：查询名字中不含有字母 R 的所有雇员： select username form employee where username not like '%R%';

例如：
select * from employee where name like '%z_';

==查询排序(order by)：==  
- asc ---升序   
- desc ---降序

按单列排序：select * from employee order by asc；  
按多列排序：select * from employee where salary>2000 order by age desc,salary asc;

==限制查询记录数(limit x,y)：==
- x---表示什么位置开始   
- y---表示查询多少条记录
例如：select * from employee limit 3,5;     //从第四条开始查询，查询五条记录

==使用集合函数查询：==  
- 1）count(字段)函数---记录有多少条数据：select count(*) from employee;
- 2）max(字段)---查询字段最大的数据：select max(*) from employee;
- 3）avg(字段)---查询字段的平均值：select avg(salary) from employee;
- 4）sum(字段)---查询字段的总数据：select sum(salary) from employee;

类型 |	含义
---|---
avg	|平均值
min|	最小值
max	|最大值
sum	|总和
count	|计数
distinct|	表示将distinct后的属性去重
group by|	将在group by上取值相同的信息分在一个组里
having|	对group by产生的分组进行筛选，可以使用聚集函数


==正则表达式查询(regexp)：==  
语法：SELECT * FROM employee5 WHERE name REGEXP 'yun$'; 

==分组查询(group by)：==  
1）group by 与 group_concat(字段) 函数一起使用：  
例如：SELECT dep_id,GROUP_CONCAT(name) FROM employee5 GROUP BY dep_id;   //按照部门id来进行分组，然后将部门相同的数据的 name 字段拼接在一起。

2）group by 与 集合函数 一起使用  
例如：select dep_id,name,svg(salary) from employee5 group by dep_id;   //使用部门id来进行分组，然后使用稽核函数avg(字段)来求得分出组的平均值。


##### 5、多表查询
==内连接：== 两个表之间有个相关联的纽带，只查询相匹配的行。  
例如：select employee.id,employee.name,employee.dept_id department.dept_name form employee,department where employee.dept_id=department.dept_id;

==外连接(left | right join)：==  
- ==外连接之左连接==：以左边为准的表的所有的值，无论右边是否匹配(右边不匹配为null)  
- ==外连接之右连接==：一右边为准的标的所有的值，无论左边是否匹配(左边不匹配为null)  
用法：SELECT 字段列表 FROM 表 1 LEFT|RIGHT JOIN 表 2 ON 表 1.字段 = 表 2.字段;     
例如：select employee.id,employee.name,employee.dept_id,department.name from employee left join department on employee.dept_id=department.dept_id;

==复合条件查询：==  
示例 1：以内连接的方式查询 employee6 和 department6 表，并且 employee6 表中的 age 字 段值必须大于 25；  
答案：select employee.id,employee.name,employee.age department.name from employee,department where employee.dept_id=department.dept_id and order by employee.age asc;

==子查询：== 将一个查询语句潜逃在另外一个查询语句中。  
- ==1、带 in 关键字的子查询：==  
例如：查询employee中的dept_id在department表中的数据：  
select * from employee where dept_id in (select * from department);
- ==2、带比较运算符的子查询==：=  !=    >=    <=   <     >     <>----不等于的意思
例如：查询employee表中age>=25的员工所有在的所有部门。  
select * from department where dept_id in (select * from employee where age>=25);  
- ==3、带exist关键字的查询==：exist关键字表示存在，这个返回的语句是真假值(true或false)，当返回真值的时候，进行后面的查询，当返回假值的时候，后面的查询停止。  
例如：查询employee表之前先判断department表是否存在dept_id=201；  
select * from employee where exists (select * from department where dept_id=201);  

**注意**：in 与exist 的区别：
- in  ---相当于指定某一区域。
- exist  ---相当于将主查询的数据，放到子查询中作为条件进行验证，根据验证结果(返回的结果是true或者false)来决定主查询的数据是否得以保留。如：select * from A where exists (select * from B where B.id = A.id);



### 六、mysql索引
#### 为什么创建索引？
目的：索引的目的是为了提高查询效率，可以类比为字典，相当于是排好序的快速查找数据结构。  

原理：在数据库系统中，维护者使用特定查找算法的数据结构，这种数据结构以某种方式指向对应的数据，从而来实现数据的高级查找算法(通常是B树索引算法)。相当于 栈里存着堆里的某个对象的地址，也相当于 图书的目录，用于快速定位数据的位置。  

#### 索引的优势与局限性？
##### 优势：
1）提高数据的检索效率，降低数据库的IO成本。  
2）通过索引列队数据进行排序，降低数据排序的成本，降低CPU的消耗。  
##### 局限性：
1）索引本身很大，所以一般索引是以索引文件的形式存在在磁盘中。  
2）数据的更新，会导致索引也必须要跟随者更新，才能保证数据的完整性，所以频繁更新的数据最好不要创建索引，数据的更新较慢。  
3）大量的表的话，需要大量的去测试，研究，调整，得到最优秀的索引。  

#### 什么情况下创建索引？
1）主键自动创建唯一索引。  
2）频繁作为查询条件的字段应该创建索引。  
3）查询中与其他关联的表的字段，外键关系建立索引。  
4）在高并发的情况下，尽量使用组合索引。  
5）使用 order by 关键字来进行某个字段排序的情况下，对这个字段创建索引的话，能够有效的提升排序效率。  

#### 什么情况下不创建索引？
1）表的记录太小，没必要建立索引。  
2）经常进行增删改的表不要创建索引。  
3）如果列的值太多相同，没必要建立索引。  

#### 索引简介：
索引在MySQL中也称为“键”，是存储引擎用于快速查找的一种数据结构。当数据量太大的情况下，创建索引能够大大的加快查询速度。通常在mysql的列上面创建索引。所以，**索引是用于快速查找排序的一种数据结构**。

#### 索引的分类：
- 单值索引---一个索引只包含一个单列，一个表可以有多个单列索引。

- 普通索引(允许字段重复)
- 唯一索引(UNIQUE)----索引列的值必须为宜，允许有空值。
- 全文索引---FULLTEXT
- 单列索引
- 多列索引---包含多列的索引
- 空间索引

#### B树索引算法：
按照特定的排序，将创建索引的列进行B树排序(也即是二叉树排序)，当使用索引查找的时候，就会使用二叉树的方式进行查找，实现快速查找。

#### 创建索引：
##### 1）在创建表的时候添加索引：
索引是使用index或者key两个关键字二选一来创建，前面可以选择索引的类型(可选的，可有可无)，后面为索引名(可有可无)，再接着是字段名，接着是长度(可有可无)。

**语法：**   
CREATE TABLE 表名 (  
字段名 1 数据类型 [完整性约束条件…],   
字段名 2 数据类型 [完整性约束条件…],   
[ unique | fulltext | spatial ]  index | key [索引名] (字段名[(长度)] [ASC |DESC]) );  

**例如：**  

```sql
create table dept1(
id int,
name varchar(20),
ppp varchar(20),
index (name)
);
```

##### 2）在表上面添加索引：
**语法：**   
CREATE [UNIQUE | FULLTEXT | SPATIAL ] INDEX 索引名  ON 表名 (字段名[(长度)] [ASC |DESC]) ;
例如：CREATE INDEX index_dept_name ON department (dept_name);  

**查看索引：**  
show create table 表明\G;

**删除索引：**  
DROP INDEX 索引名 ON 表名;

**索引口诀：**
- 索引失效的情况：  
    - 使用复合索引，索引的第一个字段必须存在，否则索引会失效。
    - 使用复合索引的顺序不能够跳过，否则会导致部分索引的失效。
    - 不能够在索引上做任何的操作，不然会导致索引的失效。
    - 使用 != 或者 <> 或者 is null 或者 is not null 的话，无法使用个索引。 
    - 使用 like 以通配符开头(如： '%abc...')的话，会导致索引失效而变成全表扫描。
    - 字符串类型必须要加单引号才能够使用索引。
    - or 关键字会导致索引失效。  
- 小表驱动大表，即小的数据集来驱动大的数据集。

##### ORDER BY排序索引优化
order by xxx，按照xxx排序，使用复合索引的第一个属性来进行排序，就会使用索引排序，更高效。



### 七、视图View
#### 1、简介：
视图相当于一张虚拟表，其内容有查询定义。试图包含一系列带有名称的列与行数据。mysql视图的作用类似于筛选。定义视图的筛选可以从当前或者其他数据库的一个或者多个表。-----将多个表的数据存放到一个视图中。

#### 2、作用：
- 使复杂的数据简单化。
- 对一些敏感的信息进行隐藏，保证安全性。

#### 3、创建视图：
##### 语法一： 
> CREATE [ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE} ]  VIEW 视图名 [(字段 1,字段 2…)] AS SELECT 语句 [WITH [CASCADED | LOCAL] CHECK OPTION ]; 

##### 语法二：
> CREATE VIEW 视图名  AS SELECT 语句;

例如：create view view_t2 as select * from t2 where id>199995;

#### 4、查看视图完整结构： 
> DESC 视图名称

#### 5、查看、更新、删除视图数据：
> select 字段 from 视图名称;    
> update 字段 from 视图名称;   
> delete 字段 from 视图名称;   

例如：select * from view_t2;   

#### 6、修改、删除视图：
> 修改视图：ALTER VIEW 视图名  AS SELECT 语句;  
> 删除视图：drop view 视图名称;  

例如：ALTER VIEW view_user AS SELECT user,password FROM mysql.user;


### 八、触发器Triggers
#### 1、简介：
触发器是一个特殊的存储过程，他的执行不是由程序调用，也不是手工调用，而是由事件来触发。当一个行为激活了触发器以后，会执行触发器的代码。
#### 2、创建触发器：

```
delimiter $$
create trigger 触发器名称 before | after 触发事件 on 表名 for each now 
begin
触发器程序体;
end$$
delimiter ;
```

##### 触发器的介绍：
触发时机：before    after

触发事件：insert   update   delete

for each now ：指的是每操作一行就执行一次触发器。

触发器程序体：需要被触发的SQL语句。


### 九、存储过程与函数（procedure and function）
#### 1、简介：
存储过程与函数是实现经过编译并存储在数据库的一段SQL语句的集合。
#### 2、与函数的区别：
1）函数必须有返回值，存储过程没有。  

2）存储过程的参数可以是in、out、inout类型，函数只能是 in 类型.

#### 3、存储过程的优点：
1）存储过程只在创建时进行编译，以后直接使用而不需要编译的过程。

2）简化复杂操作，可以结合事务一起封装。

3）复用性好，安全性高。

#### 4、创建存储过程：

```
delimiter $$ 
create procedure 过程名([形式参数列表]) 
begin
SQL 语句
end $$ 
delimiter ;
```

##### 存储过程的参数形式：
[IN | OUT | INOUT] 参数名 类型 

IN 输入参数 

OUT 输出参数 

INOUT 输入输出参数

调用：call 存储过程名(实参列表) 


#### 5、函数 function
##### 创建函数：

```
delimiter $$ 
create function 函数名(参数列表) returns 返回值类型 
begin 有效的 SQL 语句 
end$$ 
delimiter ;
```

##### 调用函数： 
select 函数名(实参列表) 


### 十、mysql安全机制 DDL  DCL
#### 1、Mysql权限表：
- 1）mysql.user --- 全局权限

- 2）mysql.db --- 数据库级别权限
- 3）mysql.tables_priv --- 表级别权限
- 4）mysql.columns_priv --- 列级别权限

#### 2、登录与退出mysql
输入语句：mysql -h192.168.21.158 -P 3306 -uroot -p123456     

**注意：**
- -h 指定主机名 【默认为 localhost】 
- -P MySQL 服务器端口 【默认 3306】 
- -u 指定用户名 【默认 root】 
- -p 指定登录密码 【默认为空密码】

#### 3、创建用户与授权：
创建一个账号：create user 'admin'@'%' identified by '123456';  
为账号赋权限：grant all on *.* to 'admin'@'%';  
授权立即生效：flush privileges;  
##### 介绍：
create---创建一个账号的关键字  
identified by '123456'---表示为创建的用户设置密码为123456；  
grant---用户授权的关键字；  
all---所有权限；  
on---指向什么库或者表；  
*.*---所有库的所有表；  
to---指向什么用户；  
'admin'---创建的用户名；  
@---表明后面跟什么ip的关键字；  
'%'---表示所有ip都可以访问；  '192.168.21.155'---表示只对这个ip地址授权；   

##### 完整的用户创建与授权介绍：   
grant 权限列表 on 库名.表名 to '用户名'@'客户端主机' [identified by ' 密码'];

例如：GRANT ALL ON *.* TO admin1@'%' IDENTIFIED BY '(123456)'; 

#### 4、删除用户：DROP USER 
例如： DROP USER 'user1'@’localhost’; 

#### 5、修改mysql密码：
##### 方法一：
mysqladmin -uroot -p'123' password 'new_password';

##### 方法二：
UPDATE mysql.user SET authentication_string=password(‘new_password’) WHERE user=’root’ AND host=’localhost’;   
FLUSH PRIVILEGES; 
##### 方法三：
登录到自己的账号，输入  SET PASSWORD=password(‘new_password’); 

#### 6、查看权限：
管理员查看他人权限----SHOW GRANTS FOR admin1@'%'\G
#### 7、回收权限：
REVOKE 权限列表 ON 数据库名 FROM 用户名@‘客户端主机’ ;  

##### 例如：
REVOKE DELETE ON *.* FROM admin1@’%’; //回收delete权限   
REVOKE ALL PRIVILEGES ON *.* FROM admin2@’%’; //回收所有权限 


### 十一、mysql日志管理
#### 1、日志分类：
- error log---错误日志     作用：用于排错   标准路径：/var/log/mysqld.log 【默认开启】

- bin log---二进制日志    作用：用于备份   
- Relay log---中继日志   作用：用于复制
- slow log---慢查询日志  作用：用于调优，记录查询操作正常情况的SQL语句。

#### 2、Error Log---错误日志
##### 注意：
1）这个日志是默认启动了的  
2）一般用于排错  
3）存放的位置默认在 /var/log/mysqld.log ，可以修改 /etc/my.cnf 文件来设置存放路径  

#### 3、Binary Log---二进制日志
##### 注意：
1）这个日志默认未启动；  
2）一般用于备份  
3）可以通过修改/etc/my.cnf文件来指定开启这个功能，并指定这个文件产生的位置。  

##### 开启bin-log：
第一步：先创建一个文件夹：mkdir /var/log/mysql-bin  
第二步：在my.cnf文件中添加：   

```
log-bin=/var/log/mysql-bin/slave2    
server-id=2
```

第三步：更改所属组与所属权限：
chown mysql.mysql /var/log/mysql-bin/  

第四步：重启mysql：  
systemctl restart mysqld


### 十二、mysql的存储引擎
#### 1）用途：
mysql的存储引擎采用的数可插拔式的存储引擎，用于实现将查询处理与其他系统任务以及数据的存储分离。常用的存储引擎为 MySAM 与 InnoDB。
#### 2）存储引擎的常用命令：
show engines;     查看所有存储引擎。  

show variables like '%storage_engine%';     查看默认存储引擎与当前正在使用的存储引擎。

#### 3）存储引擎MySAM与InnoDB的区别：
![image](https://note.youdao.com/yws/api/personal/file/B259C0BA9C3D44BA94630BBEE6B6BA9B?method=download&shareKey=93e132a92f6f6580104cfd2ce6da6726)


#### 4）MyISAM与InnerDB的着重点：
MyISAM存储引擎，因为其不支持事务，事务锁又是表锁，不适合高并发，主要的关注点是查询的效率的高效性，所有MyISAM更着重于数据的读的操作。  
InnerDB存储引擎，支持事务，支持行锁，对高并发有着很大的优势，更加适合于增删改的操作。


### 十三、SQL执行顺序
#### 计算机读取SQL的顺序：
从from开始，然后执行各种查询条件，再执行select的内容，最后读取limit关键字。

![image](https://note.youdao.com/yws/api/personal/file/68B955EB8F4A4839A1D782189B3833E2?method=download&shareKey=ed892ff403c3261afb576c635e4ae8ce)

![image](https://note.youdao.com/yws/api/personal/file/25594FB676A7495AB02FC11ADB8B57E7?method=download&shareKey=1a35974418bfd1896757e6580c571244)



### 十三、mysql性能分析
#### 1、mysql的常见瓶颈
1）CPU在饱和的时候一般发生在数据状图内存或者磁盘上读取数据的时候。

2）IO瓶颈发生在装在数据远远大于内存容量。

3）服务器硬件的性能瓶颈。

#### 2、Explain---执行计划
##### 简介：
使用Explain关键字可以来模拟优化器执行SQL查询语句，从而知道mysql是如何处理你的SQL语句的。用于分析查询语句或者表现mysql的性能瓶颈。
##### 使用方式：
explain + SQL语句


#### 3、MySQL慢查询日志
##### 简介：
mysql的慢查询日志是mysql提供的一种日志记录，它用来就在mysql中SQL语句响应时间超过阈值的SQL语句，也就是指的是运行时间超过了long_query_time的SQL语句，会被记录到慢查询日志中。

##### 操作：
查看慢查询日志功能的状态与路径位置 --- show variables like'%slow_query_log%';  

查看慢查询日志的阈值 long_query_time 的设置时间：show variables like 'long_query_time%';

设置慢查询日志功能(一次性启动，重启以后自动关闭) --- set global slow_query_log=1;

设置慢查询日志的阈值 long_query_time 的值(一次性启动，重启以后自动关闭)：
--- set global long_query_time=3;   重连即可；


设置慢查询日志功能(永久生效)的所有配置 --- 修改etc/my.cnf文件，在[mysqld]下面添加：

```
slow_query_log=1
slow_query_log_file=/var/lib/mysql/localhost-slow.log
long_query_time=3;
log_output=FILE
```

#### 4、MySQL日志分析工具 --- mysqldumpslow
得到按照条件查找出慢查询日志中的信息，并实现数据的统计与分析，使用方法：

![image](https://note.youdao.com/yws/api/personal/file/7255B6ECF08E4597A2A854B1BC43EA23?method=download&shareKey=65d994b198d3cb35ec7219b3eb239cff)


### 十四、Mysql锁机制
#### 1、分类：
##### 第一种：读锁(共享锁)：
针对同一份数据，多个度操作可以同时进行而不会互相影响。  

**注意：**  
每个账号都能够对数据进行读取，但是只有加锁的账号才能够有写的操作，其他账号如果执行写的操作，会一直等待，直到锁释放以后才会执行成功。
##### 第二种：写锁(排它锁)：
当前写的操作未完成的话，会阻断其他写与读的操作。  

**注意：**  
每个账号都不能够对数据进行读、写操作，只有枷锁的账号才能够有写的操作，其他账号如果执行读、写的操作，会一直等待，直到锁四方以后才会执行成功。

##### 第三种：行锁：
锁定某一行，锁定的颗粒力度非常小，能够降低资源争抢的现象，加大了程序的并发力度。有可能出现死锁。
##### 第四种：表锁：
锁定整张表，锁定的粒度很大，并发能力非常低。但是很好的避免了死锁现象。

##### 第五种：页锁：
锁定某一页，锁定的粒度介于行锁与表锁之间，有可能出现死锁。

#### 2、表锁---主要是适用于MyISAM存储引擎---一般用于读的操作：
特点：偏向于MyISAM存储引擎，开销小，加锁快，无死锁，锁定力度大，发生所冲突的概率高，并发度低。  
手动增加表锁：Lock table 表名字1 read/write , 表名字2 read/write;  
手动释放表锁：unLock tables;  
#### 3、行锁---主要是适用于InnoDB存储引擎---一般用于写的操作：
先关闭自动提交：set autocommit = 0 ;    
设置行锁：select * from tb_person where id = 8 for update;    为id = 8 的那一行上锁；  
....中间对这一行可执行其他操作.....   
取消行锁：commit;   

### 十五、SQL查询案例
##### 1、查询所有科目的平均成绩并按照平均成绩排序，且需要显示出平均成绩对应的科目名称？
```
select c.Cname,s.Cno,avg(s.Degree) as '平均成绩'
from score s, course c 
where s.Cno= c.Cno
GROUP BY Cno order by avg(Degree);
```



### 数据常考内容
#### 数据库优化策略？
- 选择存储范围较小的字段。
- 尽量使用关联查询代替子查询。
- 在需要上锁的时候，为了尽可能的提高并发的话，可以使用乐观锁、行锁。
- 使用explain对SQL语句进行分析，优化。
- 为经常查询的数据创建索引，实现快速查询。


#### 数据库的事务是什么？
**简介：**  
事务是由一组SQL语句组成的操作单元，事务具有四个属性，通常称为ACID：  
**原子性(Atomicity)：**  
事务是一个原子操作单元，对其数据的操作，要么全部执行，要么都不执行。  
**一致性(Consistent)：**  
事务在开始与完成时，都必须保持一致状态，这就意味着相关的数据规则都必须运用于事务，以保持数据的完整定；事务结束时，所有的内部数据结构也都必须正确。  
**隔离性(Isolation)：**  
数据库提供一定的隔离机制，保证事务不受外部因素的影响，独立的执行事务的操作。也就意味着处理过程中的中间状态对外部是不可见的。  
**持久性(Durable)：**  
事务完成之后，它对于数据的修改是永久性的，即是出现系统故障也能够保持。  


#### 并发数据事务处理带来的问题？
**更新丢失：**  
当两个或者多个事务选择同一行，然后基于最初的值进行更新该行时，由于彼此都不知道对方在堆这行就行事务的操作，所以出现问题---最后提交的事务覆盖掉了前面提交的事务的数据。  
**脏读：**    
事务A正在*修改数据*且尚未提交，事务B在事务A之后读取了事务A尚未提交的数据，在这个数据之上进行了操作。如果事务A执行回滚，事务B的数据就会无效，不符合一致性的要求。    
**幻读：**    
事务A正在*新增数据*且尚未提交，事务B在事务A之后读取了事务A尚未提交的数据，在这个数据之上进行了操作。如果事务A执行回滚，事务B的数据就会无效，不符合一致性的要求。  
**不可重复读：**   
事务A在读取部分数据以后的某个时间，再次重复读取者部分数据，结果发现数据已经被其他事务B修改过，造成了数据的不匹配，不符合事务的“隔离性”规定。


#### 数据库的三大范式？
第一范式：数据库的每一列都是一个不可分割的数据项，强调的是数据的原子性，每一列不可以有多个属性值。  

第二范式：表中的其他非主键的属性必须完全依赖于主键。  

第三范式：表中的其他非主键的属性不能依赖于其他非主键的属性(消除了第二范式的传递依赖)。


#### union 与 union all 与 Intersect 与 Minus 的区别？
**Union：**  
对两个结果集进行并集操作，不包括重复行，同时进行默认规则排序（也就是将两个结果集合在一起，去掉重复的行）。   
**Union all：**  
对两个结果集进行并集操作，包括重复行，不进行排序（也就是将两个结果集合在一起，不会去掉重复的行）。  
**Intersect：**  
对两个结果集进行交集操作，不包括重复行，不进行排序。    
**Minus：**  
对两个结果集进行差集操作，不包括重复行，同时进行默认排序规则。  

例如：   

```
select * from employee where gender = 1
union 
select * from employee where d_id = 2;
```

#### 什么是事务的隔离级别？
为了避免并发事务带来的脏读、幻读等问题，数据库提供了一定的事务隔离级别机制：  

事务的隔离级别越严格，并发的副作用越低，但是并发的能力也就越来越弱。事务的隔离级别在一定能过的程度上就是将事务进行 “串行化” 执行，所以事务的隔离是按照数据的需求来进行隔离级别的选择。   

![image](https://note.youdao.com/yws/api/personal/file/524C1E780A0549EFB7D2A28D0C823079?method=download&shareKey=aae04ec9054da9d8c83bb6e807dde731)

#### SQL中 left join、right join、inner join 的区别？
left join(左联接) 返回包括左表中的所有记录和右表中联结字段相等的记录 。  

right join(右联接) 返回包括右表中的所有记录和左表中联结字段相等的记录。  

inner join(等值连接) 只返回两个表中联结字段相等的行。  


表A记录如下：

```
aID　　　　　aNum
1　　　　　a20050111
2　　　　　a20050112
3　　　　　a20050113
4　　　　　a20050114
5　　　　　a20050115
```

表B记录如下:

```
bID　　　　　bName
1　　　　　2006032401
2　　　　　2006032402
3　　　　　2006032403
4　　　　　2006032404
8　　　　　2006032408
```

例一：left join语句：select * from A left join B  on A.aID = B.bID;  

结果如下: 显示左表全部内容，以及右表所对应的内容，右表内容不存在的话，使用 NULL代替。

```
aID　　　　　aNum　　　　　bID　　　　　bName
1　　　　　a20050111　　　　1　　　　　2006032401
2　　　　　a20050112　　　　2　　　　　2006032402
3　　　　　a20050113　　　　3　　　　　2006032403
4　　　　　a20050114　　　　4　　　　　2006032404
5　　　　　a20050115　　　　NULL　　　　　NULL
```



例二：right join语句: select * from A right join B on A.aID = B.bID  

结果如下: 显示右表全部内容，以及左表所对应的内容，左表内容不存在的话，使用 NULL代替。

```
aID　　　　　aNum　　　　　bID　　　　　bName
1　　　　　a20050111　　　　1　　　　　2006032401
2　　　　　a20050112　　　　2　　　　　2006032402
3　　　　　a20050113　　　　3　　　　　2006032403
4　　　　　a20050114　　　　4　　　　　2006032404
NULL　　　　　NULL　　　　　8　　　　　2006032408
```

例三：inner join语句: select * from A inner join B on A.aID = B.bID;  

结果如下:只显示与条件匹配的行。

```
aID　　　　　aNum　　　　　bID　　　　　bName
1　　　　　a20050111　　　　1　　　　　2006032401
2　　　　　a20050112　　　　2　　　　　2006032402
3　　　　　a20050113　　　　3　　　　　2006032403
4　　　　　a20050114　　　　4　　　　　2006032404
```



# MyCAT学习笔记

## 一、MyCAT搭建分库分表

### 1、搭建环境
准备两台虚拟机，并安装mysql，然后创建两个数据库data1、data2(这两个数据库可以在一个MySQL中，也可以分别在三个mysql中)。  

安装jdk。  

解压 MyCAT 压缩包。  

### 2、修改mycat的三大配置文件：
三大文件都在mycat的config目录下。  

- schema.xml   
- server.xml   
- rule.xml


#### 1）schema.xml   文件的配置
##### 作用：
用于配置多个数据库之间该如何分片。

```
<?xml version="1.0"?>
<!DOCTYPE mycat:schema SYSTEM "schema.dtd">
<mycat:schema xmlns:mycat="http://org.opencloudb/">
        <!-- 指定一个mycat的数据库名字为：taotao  -->
        <schema name="taotao" checkSQLschema="false" sqlMaxLimit="100">
                <!-- 指定在这个数据库中的哪一张表需要被分库分表 -->
                <table name="tb_item" dataNode="dn1,dn2" rule="auto-sharding-long" />
        </schema>
<!-- 指定分的每个库对应的是真实数据库的哪个库 -->
        <dataNode name="dn1" dataHost="mysql1" database="data1" />
        <dataNode name="dn2" dataHost="mysql2" database="data2" />
<!-- 指定需要连接的数据库的连接信息 -->
        <dataHost name="mysql1" maxCon="1000" minCon="10" balance="0"
                writeType="0" dbType="mysql" dbDriver="native" switchType="1"  slaveThreshold="100">
                <heartbeat>select user()</heartbeat>
                <writeHost host="hostM1" url="192.168.21.126:3306" user="root"
                        password="123456">
                </writeHost>
        </dataHost>
        <dataHost name="mysql2" maxCon="1000" minCon="10" balance="0"
                writeType="0" dbType="mysql" dbDriver="native" switchType="1"  slaveThreshold="100">
                <heartbeat>select user()</heartbeat>
                <writeHost host="hostM1" url="192.168.21.127:3306" user="root"
                        password="123456">
                </writeHost>
        </dataHost>
</mycat:schema>
```


#### 2）server.xml  文件的配置
##### 作用：
用于配置mycat登录账号与密码，以及账号的权限。

例如：下面是默认创建的两个账号

```
<user name="test">
        <property name="password">test</property>
        <property name="schemas">taotao</property>
</user>

<user name="user">
        <property name="password">user</property>
        <property name="schemas">taotao</property>
        <property name="readOnly">true</property>
</user>
```

#### 3）rule.xml   文件的配置
##### 作用：
用于指定 mycat 进行分片的数据的大小的设置。

### 3、启动mycat
进入mycat的 bin 目录，输入 ./mycat console 即可查看是否启动成功。



## 二、MyCAT主从复制，读写分离
### 1、MySQL主服务器的配置

#### 第一步：修改my.cnf配置文件，开启二进制日志文件：

```
#启用二进制日志
log-bin=mysql-bin
#服务器唯一ID，一般取IP最后一段
server-id=134
```

#### 第二步：重启mysql服务

```
service mysqld restart
```

#### 第三步：建立帐户并授权slave

```
mysql>GRANT FILE ON *.* TO 'backup'@'%' IDENTIFIED BY '123456';
mysql>GRANT REPLICATION SLAVE, REPLICATION CLIENT ON *.* to 'backup'@'%' identified by '123456';
```

注意：一般不用root帐号，“%”表示所有客户端都可能连，只要帐号，密码正确，此处可用具体客户端IP代替，如192.168.145.226，加强安全。

#### 第四步：刷新权限

```
mysql> FLUSH PRIVILEGES;
```


#### 第五步：查看mysql现在有哪些用户

```
mysql>select user,host from mysql.user;
```


#### 第六步：查询master的状态---再从服务中会用到图片中的参数。

```
mysql> show master status;
```
![image](https://note.youdao.com/yws/api/personal/file/5B6549669D354EDA91636D361F49C3BC?method=download&shareKey=8aeda3268eee121f62f65dc87f6c8221)


### 2、MySQL从服务器的配置
#### 第一步：修改my.conf文件

```
[mysqld]
server-id=166
```


#### 第二步：配置从服务器

```
mysql> change master to master_host='192.168.21.126',master_port=3306,master_user='root',master_password='123456',master_log_file='mysql-bin.000001',master_log_pos=883;
```

注意：语句中间不要断开，master_port为mysql服务器端口号(无引号)，master_user为执行同步操作的数据库账户，“120”无单引号(此处的120就是show master status 中看到的position的值，这里的mysql-bin.000001就是file对应的值)。

#### 第二步：启动从服务器复制功能

```
Mysql>start slave;
```


#### 第三步：检查从服务器复制功能状态：

```
mysql> show slave status
```

如果：Slave_IO_Running 与 Slave_SQL_Running 两者中存在一个或者多个No的话，证明启动不成功；  

修改方法：  
1）查看两个mysql实例是否生成的UUID相同：相同的话，则删除从服务器的/var/lib/mysql/auto.cnf文件，重新启动服务。  
2）如果还是不成功，则在从服务器上输入 stop slave;  
然后输入   SET GLOBAL SQL_SLAVE_SKIP_COUNTER=1;  
最后重启 start slave;  即可。  