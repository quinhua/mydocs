# 一、Mysql常用的字段类型

![grsegy564gyh4yh5e](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/grsegy564gyh4yh5e.5f22poyeoew0.webp)

## 1.数值类型

### 1.1整形

![jkre89345tjkfg890erd](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/jkre89345tjkfg890erd.6yvby2t1cyo0.webp)

### 1.2浮点型

![tg45yh56e4yh56uy3wyhggdr](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/tg45yh56e4yh56uy3wyhggdr.4wk2c1xfiju0.webp)

### 1.3定点型

![terterty345ygertgaeret](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/terterty345ygertgaeret.4x3k6pddfq00.webp)

## 2.字符串类型

![ewrtert34trgertewterty](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/ewrtert34trgertewterty.5ox1debr47g0.webp)

## 3.日期时间类型

![fsdf34t455t3terwtertert34tr](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/fsdf34t455t3terwtertert34tr.5dqt97eavz40.webp)

# 二、Mysql命令

## 修改my.ini配置文件

```java
暂停服务
在路径：D:\ProgramFiles\mysql\MySQLServer5.7Data 找到my.ini文件
修改内容1：
找到[mysql]命令，大概在63行左右，在其下一行添加
default-character-set=utf8
修改内容2:
找到[mysqld]命令，大概在76行左右，在其下一行添加
character-set-server=utf8
collation-server=utf8_general_ci
修改完毕后，重启MySQL服务]
```

## 查看编码命令

```sql
show variables like 'character_%';
show variables like 'collation_%';
```

## 1.基础命令

### 1. 库命令

#### 1.创建数据库

```sql
create database 数据库名 [character set 字符集 ];
```

#### 2.查看有哪些数据库

```sql
show databases;
```

#### 3.查看数据库的创建细节

```sql
show create database 数据库名;
```

#### 4.修改数据库

```sql
alter database 数据库名 character set 字符集 [collate 校对规则];
```

#### 5.切换数据库

```sql
use 数据库名;
```

#### 6.显示正在使用的数据库

```sql
select database();
```

#### 7.删除数据库

```sql
drop database 数据库名;
```

### 2.建表语句

```sql
-- 创建表(不带约束)语法 :
create table 表名 (
//属性名 属性的类型 [Java中 : 数据类型 变量名]
字段名1 字段类型(长度) [ default 默认值 ],
段名2 字段类型(长度) [ default 默认值 ],
...
) [character set 字符集];

//示例
   create table tb_employee(
      id int,
      name varchar(30),
      gender char(1),
      birthday date,
      entry_date date,
      job varchar(100),
      salary double
   );
```

### 3.查看表

#### 1.查看数据库中所有的表

```sql
show tables;
```

#### 2.查看表中字段信息

```sql
desc 表名;
```

#### 3.查看建表语句

```sql
show create table 表名;
```

### 4.删除表

```sql
drop table 表名;
```

### 5.修改表信息

```sql
-- 修改表添加字段. 给表新增一列
alter table 表名 add 字段名 类型(长度) 约束;

-- -修改表修改字段的类型长度和约束. 修改表的字段信息
alter table 表名 modify 字段名 类型(长度) 约束;

-- 修改表删除表中这个字段.
alter table 表名 drop 字段名;

-- 修改表修改字段名
alter table 表名 change 旧的字段名 新的字段名 类型(长度) 约束;

-- 修改表名
rename table 旧表名 to 新表名;

-- 修改表的字符集
alter table 表名 character set 字符集;
```

## 2.sql命令

> DML:数据操纵语言,insert、delete、update
> DQL:数据查询语言,select
> DDL:数据库定义语言,建表、建库、删库
> CRUD:Create、Retrive(取)、Update,Delete

### (1)DML:

#### 往表里添加值

```sql
-- 添加部分字段信息
insert into 表名 (字段名1,字段名2,字段名3...) values (值1,值2,值3...),(值1,值2,值3...);

-- 添加全部字段信息
insert into 表名 values (值1,值2,值3...),(值1,值2,值3...);


--添加字段的值的类型如果是字符串或者日期类型,那么在插入的值的时候,就需要使用单引号或者双引号引起来.
-- 推荐使用单引号
```

#### 修改数据

```sql
-- 修改某个字段的值 [一般会带条件,若不带条件就是把所有的值都改变]
update 表名 set 字段名 = 值,字段名=值 [where 条件];


-- 修改某个字段的值 [可以做运算]
update 表名 set 字段名 = 字段名 [赋值操作符] 值,字段名=值 [where 条件];
```

#### 删除数据

```sql
//删除所有数据,自增有记录,不支持回滚,一定要条件,不带条件全部删除
delete from 表名 where 条件; 

//删除所有数据,自增无记录,支持回滚,属于DDL
truncate table 表名; 

注意 :
delete方式属于dml,逐条记录进行删除
truncate方式属于ddl,直接删除表,然后重新创建表
```

### (2)DQL:

#### 1.编写顺序

![dfg45tg2346yshr](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/dfg45tg2346yshr.y5qei8qjiq8.webp)

#### 2.执行顺序

![gdfgerttyhjerfajuaw](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/gdfgerttyhjerfajuaw.gwx1k1upr7c.webp)

#### 3.基本查询

```sql
-- 基础查询 : * 全部列 , [列名,列名] 指定列 , distinct : 列的去重
select [distinct] * 或 [列名,列名...] from 表名;

-- 取别名 as
select 列名 as 别名,列名 as 别名 from 表名;
```

#### 4.条件查询

```sql
select [distinct] * 或 [列名,列名...] from 表名 where 条件;
```

#### 5.查询中的逻辑运算符

```sql
查询中的逻辑运算符 :
    > ,< ,>= ,<= ,= ,<>
    in : 一组数据内 in(值1,值2,值3...)
    like : 模糊查询.
           _ : 一个字符 where name like '张_'; -- 名字是2个字,且开头为张
           % : 任意个字符 where name like '张%'; -- 名字是n个字,且开头为张
               张% : 以张开头
               %张 : 以张结尾
               %张% : 包含张即可
    and : 逻辑与 -> 并且
    or : 逻辑或 -> 或者
    not : 逻辑非
    between ... and : 在xxx之间
```

#### 6.带排序功能的查询

```sql
select * from 表 where 条件 order by 列名 asc/desc,列名 asc/desc;

asc : 升序 -- 默认
desc : 降序 
```

#### 7.聚集函数

> 聚集函数 : SQL提供的方法

##### 1.统计函数

```sql
count(字段):统计表中记录的个数.

语法:
    select count(*) from 表名;
```

##### 2.求和函数

```sql
sum() :求和

语法: 
    select sum(列名) from 表名;
```

##### 3.平均值函数

```sql
avg():求平均值


语法: 
    select avg(列名) from 表名;
```

##### 4.最值函数

```sql
max(): 求最大值
min(): 求最小值

语法: 
    select max(列名) from 表名;
    select min(列名) from 表名;
```

#### 8.分组查询

```sql
select * from 表 where 条件 group by 列名;

-- 注意事项:
1. where 后面的条件不能使用聚集函数.
2. having 分组后的条件过滤. [having后面跟聚集函数]
```

#### 9.分页查询

```sql
-- 语法
select * from 表名 limit begin , pageSize;

-- begin : 开始索引
-- currentPage : 当前页数
-- pageSize : 查询记录个数

begin = (currentPage-1) * pageSize;

-- 实现对tb_orders的分页查询(每页2行数据)
-- 第一页
select * from tb_orders limit 0,2;
-- 第二页 
select * from tb_orders limit 2,2;
-- 第三页 
select * from tb_orders limit 4,2;

[注:]总页数公式：totalRecord是总记录数；pageSize是一页分多少条记录  
int totalPageNum = (totalRecord +pageSize - 1) / pageSize;
```

![fgh8ghedr67509gc345trg](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/fgh8ghedr67509gc345trg.390p7qxljga0.webp)

#### 10.账户管理(了解)

> 注意  :  账户管理、授权管理只能在root账户下。

```sql
-- 查询账户
use mysql;
-- User:账户名
-- Host:允许账户登录mysql服务器的主机地址
select User,Host from user;

-- 添加账户
create user '账户名'@'主机名' identified by '密码';

-- 删除账户
drop user '账户名'@'主机名';

-- 修改账户
set password for '账户名'@'主机名' = password('新密码'); 
```

#### 11.权限管理(了解)

> mysql账户可以拥有不同的权限

```sql
-- 查看授权
show grants for '账户名'@'主机名';

-- 添加授权
grant 权限1 , 权限2 ,... on 数据库名.表名 to '账户名'@'主机名';

-- 撤销授权
revoke 权限1 , 权限2 , ... on 数据库名.表名 from '账户名'@'主机名';
```

#### 12.约束

```sql
约束概述 : 对表中的数据（记录）进行限定，保证数据的正确性、有效性、完整性。
```

| 约束名称           | 说明      |
|:--------------:|:-------:|
| PRIMARY KEY    | 主键约束    |
| AUTO_INCREMENT | 主键、自动增长 |
| UNIQUE         | 唯一约束    |
| NOT NULL       | 非空约束    |
| FOREIGN KEY    | 外键约束    |

##### 1.主键约束

```sql
-- 概述
1 . 非空和唯一两个功能
2 . 一张表只能有一个列作为主键
3 . 主键一般用于表中数据的唯一标识

-- 语法

-- 建表时添加主键约束
    create table 表名(
        字段名1 字段类型(长度) primary key ,
        字段名2 字段类型(长度)
        ...
    );

--  删除主键约束
alter table 表名 drop primary key;

-- 建表后添加主键约束
alter table 表名 modify 字段名 字段类型(长度) primary key;
```

##### 2.自增约束

```sql
-- 概述
1 . 如果主键是一个整数类型，那么就可以将其设置为自增约束；
2 . 值可以自动增长。

-- 语法
-- 创建表时添加自增约束
create table 表名(
    字段1 字段类型(长度) primary key auto_increment,
    字段2 字段类型(长度),
    ...
);

-- 删除自增约束
alter table 表名 modify 字段1 字段类型(长度);

-- 创建表后添加自增约束
alter table 表名 modify 字段1 字段类型(长度) auto_increment;
```

##### 3.唯一约束

```sql
-- 概述
    使用唯一约束的字段的值不可重复

-- 语法
-- 创建表时添加唯一约束
create table 表名(
    字段1 字段类型(长度) unique,
    字段2 字段类型(长度),
    ...
);

-- 删除唯一约束
alter table 表名 drop index 字段名;

-- 创建表后添加唯一约束
alter table 表名 modify 字段名 字段类型(长度) unique;
```

##### 4.非空约束

```sql
-- 概述
    使用唯一约束的字段的值不可为null

-- 语法
-- 创建表时添加非空约束
create table 表名(
    字段1 字段类型(长度) not null,
    字段2 字段类型,
    ...
);

-- 删除非空约束
alter table 表名 modify 字段 字段类型(长度);

-- 创建表后添加非空约束
alter table 表名 modify 字段 字段类型(长度) not null;
```

##### 5.默认值约束

```sql
-- 概念 
    给指定字段一个默认值

-- 语法
-- 创建表时添加默认值约束
create table 表名(
    字段1 字段类型(长度) default 默认值,
    字段2 字段类型(长度),
    ...
);

-- 删除默认值约束
alter table 表名 modify 字段  字段类型(长度);

-- 创建表后添加默认值约束
alter table 表名 modify 字段  字段类型(长度) default 默认值;
```

##### 6.检查约束(了解)

```sql
-- 概述
1 . 对字段的值的范围进行检查；
2 . mysql不支持检查约束。

-- 语法 [sql不支持]
create table tb_student (
    id int primary key auto_increment,
    name varchar(30),
    gender char(1) check ('男' or '女')
);

-- 代替
create table tb_student(
    id int primary key auto_increment,
    name varchar(30),
    gender enum ('男','女')
);

-- 插入数据
insert into tb_student values (null,'张三','男');
-- 可以实现的[check的情况下,在枚举的情况下是不能实现的]
insert into tb_student values (null,'李四','妖'); 
```

#### 13.数据库备份和还原

```sql
-- 命令语法
    --备份
    mysqldump -uroot -p 数据库名称 > 路径地址\文件名.sql
    mysqldump -uroot -p mydb1 > 路径地址\文件名.sql
    --还原
    mysql -uroot -p 数据库名称 < 路径地址\文件名.sql
    mysql -uroot -p mydb1 < 路径地址\文件名.sql
```

#### 14.多表查询

##### 1.内连接查询(重要)

```sql
-- 概述
    获取的结果是两张表的交集

-- 语法
    显式内连接 : select * from 表A [inner] join 表B on 条件; [推荐写法 ： 性能更好]
    隐式内连接 : select * from 表A , 表B where 条件;
```

##### 2.外连接查询

```sql
-- 概述
    获取的结果是两张表的交集和某一张表全部

-- 语法
    左外链接 : 两张表的交集和左边表的全部
        select * from 表A left [outer] join 表B on 条件;
    右外链接 : 两张表的交集和右边表的全部
        select * from 表A right [outer] join 表B on 条件;
```

##### 3.union(了解)

```sql
-- 概述
    UNION 操作符用于合并两个或多个 SELECT 语句的结果集。

-- 语法
    UNION : 求两个select结果的并集，会去重
        select * from 表1 union select * from 表2;

    UNION ALL : 求两个select结果的并集，不会去重
        select * from 表1 union all select * from 表2;
```

##### 4.子查询

```sql
-- 概述
    指的是一条语句的查询结果需要依赖另一条语句的查询结果.

-- 语法
    -- any 任意一个
    select * from 表A where 条件 > any(sql查询语句); 
    -- all 所有
    select * from 表A where 条件 > all(sql查询语句);
```

#### 15.函数

```sql
-- 概述
  - MySQL 包含了大量并且丰富的函数，这些函数会对传递进来的参数进行处理，并返回一个 处理结果。
-- 分类
  - 多行函数
    - 根据多条记录返回一个结果
    - sum()、avg()、count()、max()、min()...
  - 单行函数
    - 根据一条记录返回一个结果
    - 字符串函数、数字函数 、日期函数 、流程函数
```

##### 1.数字函数

```sql
# ABS(X) : 返回X的绝对值
select ABS(-100);

# CEIL(X) : 向上取整
select CEIL(1.1);

# FLOOR(X) : 向下取整
select FLOOR(1.9);

# MOD(X,Y) : 取X对Y的模
select MOD(3,4);

# RAND() : 取0~1随机数
select RAND();

# ROUND(X,Y) : 对X的Y位小数进行四舍五入
select ROUND(3.1235,1);

# TRUNCATE(X,Y) : 截断X，保留Y位小数
select TRUNCATE(3.12345,2);

# SQRT(X) : 对X进行开方
select SQRT(4);

# POW(X,Y) : 求X的Y次幂
select POW(2,3);
```

##### 2.字符串函数

```sql
# CONCAT(str1,str2) : 将str1、str2拼接成一个字符串
select CONCAT('a','b','c');

# CONCAT_WS(separator,str1,str2) : 将str1、str2拼接成一个字符串，str1和str2之间用separator
select CONCAT_WS('-','a','b','c');

# CHAR_LENGTH(str) : 获取str的字符长
select CHAR_LENGTH("林扬生");

# LENGTH(str) : 获取str的字节长度
select LENGTH("林扬生");

# INSERT(str,pos,len,newstr) : 将str从pos位置开始的len长度的子字符串用newStr替换
select INSERT('helloworld',1,5,'Java');

# UPPER(str) : 将str转大写
select UPPER('hellomysql');

# LOWER(str) : 将字符串中所有字符转为小写
select LOWER('ABC');

# LEFT(str,len) : 返回str左边len个字符
select LEFT('hellomysql',5);

# LPAD(str,len,padstr) : 使用padstr对str的左边进行填充，直到达到len长度
SELECT LPAD("mysql",20,"hello");
SELECT RPAD("hello",20,"mysql");

# LTRIM(str) : 去除str左边空白
select LTRIM('   hello   ');
select RTRIM('   hello   ');
select TRIM('   hello   ');

# TRIM(BOTH remstr from str) : 将str开头和结尾的remstr都去除
select TRIM(BOTH 'a' from 'ahellomysqla');

# TRIM(LEADING remstr from str) : 将str开头的remstr都去除
select TRIM(LEADING 'a' from 'ahellomysqla');

# TRIM(LEADING remstr from str) : 将str结尾的remstr都去除
select TRIM(TRAILING 'a' from 'ahellomysqla');

# REPEAT(str,count) : 将str重复count次
select REPEAT('ab',3);

# REPLACE(str, oldstr , newstr) : 将str中的oldstr替换成newstr
select REPLACE("hellomysql","l","L");

# SUBSTRING(str, begin, end) : 将str从begin处开始到end处截取
select SUBSTRING('ahellomysqla', 2, 11);
```

##### 3.日期函数

```sql
# CURDATE() : 获取当前日期(年-月-日)
select CURDATE();

# CURTIME() : 获取当前时间(时分秒)
select CURTIME();

# NOW() : 获取当前日期时间(年-月-日 时:分:秒)
select NOW();

# YEAR(date) : 获取日期的年份
select YEAR(NOW());

# DAYOFWEEK(date) : 返回周几,周日是1,周六是7
select DAYOFWEEK(NOW());

# DAYNAME(date) : 返回周几的名称，比如 : Monday
SELECT DAYNAME(NOW());

# DATEDIFF(expr1,expr2) : 获取expr2和expr1的间隔天数
select DATEDIFF(CURDATE(),'1999-1-1');

# DATE_FORMAT(date,format) : 将date对象转换为日期字符串
# xxxx年xx月xx日 xx时xx分xx秒 xx
select DATE_FORMAT(NOW(),'%Y年%m月%d日 %H时%i分%s秒 %W');

# STR_TO_DATE(str,format) : 将str按照format转换为date
SELECT STR_TO_DATE("2021年10月23日 09时47分21秒",'%Y年%m月%d日 %H时%i分%s秒');
```

##### DATE_FORMAT(datetime,fmt) 和 STR_TO_DATE(str, fmt)

| 格式符 | 说明                                  | 格式符   | 说明                                  |
| --- | ----------------------------------- | ----- | ----------------------------------- |
| %Y  | 4位数字表示年份                            | %y    | 表示两位数字表示年份                          |
| %M  | 月名表示月份（January,....）                | %m    | 两位数字表示月份（01,02,03。。。）               |
| %b  | 缩写的月名（Jan.，Feb.，....）               | %c    | 数字表示月份（1,2,3,...）                   |
| %D  | 英文后缀表示月中的天数（1st,2nd,3rd,...）        | %d    | 两位数字表示月中的天数(01,02...)               |
| %e  | 数字形式表示月中的天数（1,2,3,4,5.....）         |       |                                     |
| %H  | 两位数字表示小数，24小时制（01,02..）             | %h和%I | 两位数字表示小时，12小时制（01,02..）             |
| %k  | 数字形式的小时，24小时制(1,2,3)                | %l    | 数字形式表示小时，12小时制（1,2,3,4....）         |
| %i  | 两位数字表示分钟（00,01,02）                  | %S和%s | 两位数字表示秒(00,01,02...)                |
| %W  | 一周中的星期名称（Sunday...）                 | %a    | 一周中的星期缩写（Sun.，Mon.,Tues.，..）        |
| %w  | 以数字表示周中的天数(0=Sunday,1=Monday....)   |       |                                     |
| %j  | 以3位数字表示年中的天数(001,002...)            | %U    | 以数字表示年中的第几周，（1,2,3。。）其中Sunday为周中第一天 |
| %u  | 以数字表示年中的第几周，（1,2,3。。）其中Monday为周中第一天 |       |                                     |
| %T  | 24小时制，%H:%i:%s                      | %r    | 12小时制                               |
| %p  | AM或PM                               | %%    | 表示%                                 |

数据库设计范式[了解]

```sql
-- 概述
 设计数据库时,需要遵循的一些规范. [后续的规范要依赖前面的规范]

-- 三大范式 
    - 第一范式 : 1NF 
        每一列都是不可以分割的项[原子项]
    - 第二范式 : 2NF
        在1NF的基础上,保留完全函数依赖
    - 第三范式 : 3NF
        在2NF的基础上,消除传递函数依赖

-- 函数依赖
    - 完全函数依赖
    A -> B 如果A是一个属性组[多个属性],则B属性值的确定需要依赖A属性组中的每一个属性值; 
        例如 : A 属性组 -> (学号,课程名称) B 属性值 ->  分数

    - 部分函数依赖
    A -> B , 如果A是一个属性值,则B属性值只需要依赖A属性组中的某一些值即可;
        例如 : A 属性组 -> (学号 , 课程名称) B 属性值 -> 姓名

    - 传递函数依赖
    A -> B , B -> C 如果通过A属性(属性组)的值,可以确定唯一B属性值,在通过B属性值可以确定唯一C属性值,则称C属性值 传递函数 依赖 A属性值;
        举例 : 学号 -> 系名 , 系名 -> 系主任

    - 码
    如果在一张表中,一个属性或者属性组,被其他的所有属性完全函数依赖,则称这个属性(属性组)为该表码
```

## 3.事务管理概述

```sql
-- 事务管理 : 可以理解成数据库的多线程 [一个事务就是一个线程]

概述 :
    一条或者多条SQL语句组成的一个执行单元叫事务,其特点是这个单元要同时成功,要么同时失败,单元中的每条SQL语句都相互依赖形成一个整体. 
    如果这个事务中有一条SQL语句执行失败或者出错了,那么整个单元都会 '回滚'[回到过去] 到事务的最初状态;
    如果事务中所有的SQL全部执行成功,则事务就顺利执行;
```

### 1.事务管理

```sql
事务的语法

-- 开启事务 , 数据库操作就变成临时操作
start transaction;

-- 操作代码

-- 回滚事务, 将数据回滚到开启事务之前的状态 --> 出现问题解决问题的方案
rollback;

-- 提交事务, 将临时数据永久保存到数据库中 --> 没有问题
commit;
```

### 2.事务的四大特征[ACID]

```sql
1. 原子性 : atomicity
    事务是不可以分割的单元,要么一起成功,要么一起失败;

2. 一致性 : consistency    
    事务操作前后,数据的总量不变 [比如转账前后,张三和李四的中金额都是2w]

3. 隔离性 : isolation
    事务和事务之间是相对独立的,独立性取决于 '隔离级别'

4. 持久性[序列化] : durability
    事务回滚或者提交后,数据将永久保存到数据中
```

### 3.事务管理概述

```sql
-- 事务管理 : 可以理解成数据库的多线程 [一个事务就是一个线程]

概述 :
    一条或者多条SQL语句组成的一个执行单元叫事务,其特点是这个单元要同时成功,要么同时失败,单元中的每条SQL语句都相互依赖形成一个整体. 
    如果这个事务中有一条SQL语句执行失败或者出错了,那么整个单元都会 '回滚'[回到过去] 到事务的最初状态;
    如果事务中所有的SQL全部执行成功,则事务就顺利执行;
```

#### 展示代码

```sql
-- 提前创建tb_user表 [属性 : id,name,money]

-- 转账业务 : 执行单元

-- 出账
UPDATE tb_user set money = money - 1000 WHERE name = 'zhangsan';

-- 在出账后,入账前出现问题
出错了

-- 入账
UPDATE tb_user set money = money + 1000 WHERE name = 'lisi';
```

#### 事务管理演示

```sql
事务的语法

-- 开启事务 , 数据库操作就变成临时操作
start transaction;

-- 操作代码

-- 回滚事务, 将数据回滚到开启事务之前的状态 --> 出现问题解决问题的方案
rollback;

-- 提交事务, 将临时数据永久保存到数据库中 --> 没有问题
commit;
```

### 4.事务的四大特征[ACID]

```sql
1. 原子性 : atomicity
    事务是不可以分割的单元,要么一起成功,要么一起失败;

2. 一致性 : consistency    
    事务操作前后,数据的总量不变 [比如转账前后,张三和李四的中金额都是2w]

3. 隔离性 : isolation
    事务和事务之间是相对独立的,独立性取决于 '隔离级别'

4. 持久性[序列化] : durability
    事务回滚或者提交后,数据将永久保存到数据中
```

### 5.事务的隔离级别

| 隔离级别             | 说明   | 存在的问题                |
| ---------------- | ---- | -------------------- |
| READ UNCOMMITTED | 读未提交 | 脏读,不可重复读,幻读          |
| READ COMMITTED   | 读已提交 | 不可重复读,幻读             |
| REPEATABLE READ  | 可重复读 | 幻读                   |
| SERIALIZABLE     | 串行化  | 以上问题都解决,伴随着新的问题: 效率低 |

> MySQL的默认隔离级别 : REPEATABLE READ [可重复读]

#### 存在的问题

```sql
脏读 : 一个事务可以获取到另一个事务中未提交的数据; 
    -- 例如 : 张三给李四赚钱[开启了事务(开启事务的特点导致数据库操作变成了临时操作)],李四确读到了张三的操作,结果张三把事务回滚了(回滚到事务最开始的时候[没有转账的时候])

不可重复读 : 一个事务读取了另一个事务中的记录,且此记录的前后数据不一致. 
    -- 例如 : 财务统计,会计扎帐 [解决方案 : 当会计扎帐后,不再执行报销操作,要报销等到下个月再报销]

幻读 : 
    1. 查询sql中的某条记录,查询结果不存在,准备插入此条数据,但是插入时确报错[说数据已经存在]    
    2. 查询sql中的某条记录,查询结果不存在,接下来删除此记录时,又显示删除成功

效率低 : 事务之间是串联执行 [B事务的执行要等到A事务执行完毕后才能执行]    
```

#### 隔离级别的操作

```sql
语法 :
    -- 设置全局隔离级别,需要重新连接数据库才能生效,永久修改 [慎用]
    set global transaction isolation level 隔离级别;

    -- 设置会话隔离级别,不需要重新连接数据库才生效[立即生效],临时修改
    set session transaction isolation level 隔离级别;

-- 查询隔离级别
    select @@tx_isolation;
```

### 6.脏读的发生和解决[了解]

```sql
脏读 : 一个事务可以读到另一个事务中未提交的数据

开发步骤 :
    1. 新建两个会话
    2. 将两个会话的隔离级别设置为 : READ UNCOMMITTED;
    3. 在会话一中开启事务,执行转账,但是不提交事务[也不会滚]
        -- 开启事务 , 数据库的操作变成了临时操作
        START TRANSACTION;

        -- 出账 
        UPDATE tb_user set money = money - 1000 WHERE name = 'zhangsan';

        -- 入账
        UPDATE tb_user set money = money + 1000 WHERE name = 'lisi';

        -- 结束事务的两个操作 : 要么回滚要么提交
        -- 回滚
        ROLLBACK;

        -- 提交
        COMMIT;    
    4. 在会话二中开启事务,查询账户信息    
        -- 开启事务 , 数据库的操作变成了临时操作
           START TRANSACTION;

        -- 查询数据 : 如果会话二中读到了会话一的临时操作 : 脏读出现
            SELECT * FROM    tb_user;
        -- 当李四出现脏读,可能把欠条已经签好; 结果张三回滚事务

         -- 回滚
            ROLLBACK;

        -- 提交
               COMMIT;    
    5. 关闭会话中所有的事物

-- 脏读问题出现了: 如何解决[把会话的隔离级别设置为 READ COMMITTED]
    重复执行3,4,5三个步骤就不会出现问题

注意 : 只需要设置会话二中的隔离级别即可 [为了大家好理解,所以会话1和会话2的隔离级别都设置]    
```

### 7.不可重复读的发生和解决[了解]

```sql
-- 概述 :
    一个事务读取另一个事务中的记录,前后的数据不一致; [财务扎帐]

-- 开发步骤 :
    1. 新建两个会话
    2. 将两个会话的隔离级别设置为 READ COMMITTED;
    3. 在会话一中 : 开启事务,执行转账操作,不提交事务 [数据没有改变,临时操作]
    4. 在会话二中 : 开启事务,执行查询操作(一次事务中的第一次查询) 
        --> [张三: 10000 李四: 10000] 肯定读不到临时操作,读到了叫脏读
    5. 在会话一中 : 提交事务 [会话一的事务完成,钱发生改变]
    6. 在会话二中 : 执行查询操作(一次事务中的第二次查询) 
        --> 因为事务一提交了,所以钱发生改变 [张三:9000 李四: 11000] 
        --> 会话二中一次事务读相同数据有2种结果

-- 解决方案 
    将两个会话的的隔离级别设置为 : REPRATABLE READ [其实只需要对会话二操作即可]
```

#### 幻读的发生和解决[了解]

```      
-- 概念 
    读的时候显示没有,但是添加时显示有
    读的时候显示没有,但是删除时显示有

-- 开发步骤 
    1. 新建两个会话
    2. 将两个会话隔离级别设置为 REPEATABLE READ;
    3. 会话一 ,开启事务 ,添加新记录,不提交事务 [临时数据]
        -- insert into tb_user values(3,'wangwu',10000);
    4. 会话二, 开启事务 , 查询记录 [查询id = 3] -- 显示查不到
        -- select * from tb_user where id = 3;
    5. 会话二, 添加记录[添加id = 3] -- 添加不进去
        -- insert into tb_user values(3,'wangwu',10000);
        -- 效果: 会话二程序无法停止, 显示有 [必须要等到会话一种的事务结束后,事务二中的程序才能停止]

-- 解决方案 
    将两个会话的隔离级别设置为 SERIALIZABLE [串行化] -- 类似于多线程中的同步锁

-- 注意事项 
    1. 串行化操作的效率太低 -- Java中多线程同步操作的效率很低
    2. 我们没有必要把隔离级别设置为 串行化 !
        why :  刚才幻读的时候,数据有没有出现问题 [id = 3 这个数据没有添加成功, 既然没有添加成功,那么事务的安全性是已经得到了保障!]

    幻读问题 : 是查询结构矛盾的问题,不会影响数据的值; 数据的安全性就得到了保障!    
```
