# MySQL

## 1.1为什么学习MySQL

- 岗位需求

- 大数据时代

- 存数据

- 数据库是所有软件体系中最核心的存在

## 1.2什么是数据库

数据库(DB、DataBase)

概念：数据仓库，软件，安装在操作系统(windows,linux,mac os)之上。SQL，存储大量的数据。500w

左右：存储数据、管理数据

## 1.3数据库分类

- (SQL)关系型：MySQL、Oracle、SQL Server、DB2、SQL lite

- 通过表与表之间，行与列之间的关系进行数据存储，

- (NoSQL not only)非关系型

- Redis、MONGODB

对象存储，通过对象自身的属性来决定

DBMS 数据库管理系统

数据库管理软件，科学有效的管理数据

## 1.4MySQL简介

关系型数据库管理系统

MySQL、开源的关系型数据库系统

体积小、速度快

5.7稳定

8.0

## 1.5MySQL安装

## 2操作数据库

## 2.1数据库的列类型

| 类型      | 备注                     | 数据长度 |
| --------- | ------------------------ | -------- |
| tinyint   | 十分小的数据             | 1字节    |
| smallint  | 较小的数据               | 2字节    |
| mediumint | 中等大小的数据           | 3字节    |
| int       | 标准的数据               | 4字节    |
| bigint    | 较大的数据               | 8字节    |
| float     | 浮点数                   | 4字节    |
| double    | 浮点数                   | 8字节    |
| decimal   | 字符串形式的浮点数(精度) | 8字节    |
| char      | 固定大小的字符串         | 0-255    |
| varchar   | 可变长的字符串           | 0-65535  |
| tinytext  | 微型文本                 | 2^8-1    |
| text      | 大文本                   | 2^16-1   |
| date      | YYYY-MM-DD               |          |
| time      | HH:mm:ss                 |          |
| datetime  | YYYY-MM-DD HH:mm:ss                |          |
| timeStamp | 时间戳 1970.1.1到现在的毫秒数 |          |
| year | 年份表示 |          |
| null | 没有值，不要使用null进行运算 |          |

## 2.2数据库的字段属性

unsigned：无符号数(声明该列不能为负数)

zerofill:零填充(不足的位数使用0填充)

自增:自动在上一条记录的基础上+1(默认),通常用来设计数据表的唯一主键~index，必须是整数类型。可自定义起始值和步长。

非空：not null 不能为空，不赋值的情况下会报错。

默认：不填写值会默认填充。

## 2.3数据库设计必须要有的字段

主键

version 乐观锁

is_delete 伪删除

gmt_create 创建时间

gmt_update 修改时间

int(M)数据库中M指示最大显示宽度，最大有效显示宽度是255。显示宽度和存储大小或类型包含的值的范围无关。int还是4个字节的长度占用。

## 2.4扩展

```mysql
Auto_increment 自增

comment 注释

show create Table student 查看创建数据表的语句

DESC student 显示表结构



```

## 2.5数据库引擎

- INNODB 默认使用
- MYISAM 早些年使用

|              | MYISAM | INNODB                 |
| ------------ | ------ | ---------------------- |
| 事务         | 不支持 | 支持                   |
| 数据行锁定   | 不支持 | 支持                   |
| 外键         | 不支持 | 支持                   |
| 全文索引     | 支持   | 不支持                 |
| 表空间的大小 | 较小   | 较大，约为MYISAM的两倍 |

常规使用操作

+ MYISAM 节约空间，速度较快

+ INNODB 安全性高，事务的处理,多表多用户操作

  >  物理空间

数据库的data目录下,一个文件夹就对应一个数据库。

- INNODB 在数据库中 *.frm文件，以及上级目录下的ibdata1

- MYISAM 对应的文件 

- *.frm表结构

- *.MYD 数据文件

- *.MYI 索引文件

  设置数据库表的字符集编码`CHARSET=utf8` MySQL默认编码是Latin1，不支持中文。

# 数据库查询语句

## 4.1查询所有信息

```mysql
select * from user;
-- 查询指定的字段
select name from user;
-- 别名
select name As 名字 user;
select name As 名字 user As 用户表
-- 函数 Concat
select CONCAT("姓名:",name) As 新名字 from user;
```

> 有时候数据库列名不能直观的表达含义，可以使用as取别名

- 去重 distinct 查询结果中重复的数据只显示一条

```mysql
select version();
select 100*3-1 as 计算结果;
select @@auto_increment_increment; -- 查询自增的步长
```

数据库中的表达式：文本值，列，NULL，函数，计算式，变量...

## 4.2连表查询

![七种连接查询](http://cdn.liwenbo.cool/seven_jion.jpg)

| inner join | 如果表中至少有一个匹配，就返回行           |
| ---------- | ------------------------------------------ |
| left join  | 会从左表中返回所有的值，即使右表中没有匹配 |
| right join | 会从右表中返回所有的值，即使左表中没有匹配 |

# MYSQL函数

## 5.1常用函数

```mysql
select abs(-8); -- 8 绝对值
select ceiling(9.4); -- 10 向上取整
select floor(9.4); -- 9 向下取整
select rand(); -- 随机数 [0-1)
select sign(5); -- 1 | 0->0 | -10 -> -1 | 10->1
-- 字符串
select char_length('嘻嘻嘻嘻嘻嘻'); -- 6 字符串长度
select concat('I','like'); -- Ilike 拼接字符串
```






