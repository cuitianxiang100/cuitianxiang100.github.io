---
layout:     post
title:      "Python笔记day46（MySQL）|索引、limit分页、慢日志"
subtitle:   "索引、limit分页、慢日志"
date:       2018-09-02 16:47:00
author:     "崔天祥"
header-img: "img/post-bg-os-metro.jpg"
catalog:    true
tags:
    - Python
    - 编程
    - MySQL
---

> 索引、limit分页、慢日志 | 若本文不能正常显示 [点击这里](https://blog.csdn.net/qq_34755081/article/details/82316526)

#### 1，索引

索引，是数据库中专门用于帮助用户快速查询数据的一种数据结构。类似于字典中的目录，查找字典内容时可以根据目录查找到数据的存放位置，然后直接获取即可。
**MySQL中常见索引有：**

普通索引
唯一索引
主键索引
组合索引
###### 1）普通索引

普通索引仅有一个功能：加速查询

```
create table in1(
    nid int not null auto_increment primary key,
    name varchar(32) not null,
    email varchar(64) not null,
    extra text,
    index ix_name (name)
)
```

<!-- more -->

```
create index index_name on table_name(column_name)

drop index_name on table_name;

show index from table_name;
```

注意：对于创建索引时如果是BLOB 和 TEXT 类型，必须指定length。


```
create index ix_extra on in1(extra(32));
```

###### 2）唯一索引

唯一索引有两个功能：加速查询 和 唯一约束（可含null）


```
create table in1(
    nid int not null auto_increment primary key,
    name varchar(32) not null,
    email varchar(64) not null,
    extra text,
    unique ix_name (name)
)
```


```
create unique index 索引名 on 表名(列名)

drop unique index 索引名 on 表名
```

###### 3）主键索引

主键有两个功能：加速查询 和 唯一约束（不可含null）


```
create table in1(
    nid int not null auto_increment primary key,
    name varchar(32) not null,
    email varchar(64) not null,
    extra text,
    index ix_name (name)
)

OR

create table in1(
    nid int not null auto_increment,
    name varchar(32) not null,
    email varchar(64) not null,
    extra text,
    primary key(ni1),
    index ix_name (name)
)
```



```
alter table 表名 add primary key(列名);

alter table 表名 drop primary key;
alter table 表名  modify  列名 int, drop primary key;
```

###### 4）组合索引

组合索引是将n个列组合成一个索引

其应用场景为：频繁的同时使用n列来进行查询，如：where n1 = 'alex' and n2 = 666。

```
create table in3(
    nid int not null auto_increment primary key,
    name varchar(32) not null,
    email varchar(64) not null,
    extra text
)
```


```
create index ix_name_email on in3(name,email);
```

如上创建组合索引之后，查询：

```
name and email  -- 使用索引
name                 -- 使用索引
email                 -- 不使用索引
```

注意：对于同时搜索n个条件时，组合索引的性能好于多个单一索引合并。


----------


#### 2，索引补充
###### 1）索引
　　索引是表的目录，在查找内容之前可以先在目录中查找索引位置，以此快速定位查询数据。对于索引，会保存在额外的文件中。

###### 2）索引种类

普通索引：仅加速查询
唯一索引：加速查询 + 列值唯一（可以有null）
主键索引：加速查询 + 列值唯一 +　表中只有一个（不可以有null）
组合索引：多列值组成一个索引，
              专门用于组合搜索，其效率大于索引合并
全文索引：对文本的内容进行分词，进行搜索 
索引合并，使用多个单列索引组合搜索
覆盖索引，select的数据列只用从索引中就能够取得，不必读取数据行，换句话说查询列要被所建的索引覆盖

###### 3）相关命令

```
- 查看表结构
    desc 表名
 
- 查看生成表的SQL
    show create table 表名
 
- 查看索引
    show index from  表名
 
- 查看执行时间
    set profiling = 1;
    SQL...
    show profiles;
```

###### 4）使用索引和不使用索引

由于索引是专门用于加速搜索而生，所以加上索引之后，查询效率会快到飞起来。
 

```
# 有索引
mysql> select * from tb1 where name = 'wupeiqi-888';
+-----+-------------+---------------------+----------------------------------+---------------------+
| nid | name        | email               | radom                            | ctime               |
+-----+-------------+---------------------+----------------------------------+---------------------+
| 889 | wupeiqi-888 | wupeiqi888@live.com | 5312269e76a16a90b8a8301d5314204b | 2016-08-03 09:33:35 |
+-----+-------------+---------------------+----------------------------------+---------------------+
1 row in set (0.00 sec)
 
# 无索引
mysql> select * from tb1 where email = 'wupeiqi888@live.com';
+-----+-------------+---------------------+----------------------------------+---------------------+
| nid | name        | email               | radom                            | ctime               |
+-----+-------------+---------------------+----------------------------------+---------------------+
| 889 | wupeiqi-888 | wupeiqi888@live.com | 5312269e76a16a90b8a8301d5314204b | 2016-08-03 09:33:35 |
+-----+-------------+---------------------+----------------------------------+---------------------+
1 row in set (1.23 sec)
```

###### 5）正确使用索引

数据库表中添加索引后确实会让查询速度起飞，但前提必须是正确的使用索引来查询，如果以错误的方式使用，则即使建立索引也会不奏效。
即使建立索引，索引也不会生效：


```
- like '%xx'
    select * from tb1 where name like '%cn';
- 使用函数
    select * from tb1 where reverse(name) = 'wupeiqi';
- or
    select * from tb1 where nid = 1 or email = 'seven@live.com';
    特别的：当or条件中有未建立索引的列才失效，以下会走索引
            select * from tb1 where nid = 1 or name = 'seven';
            select * from tb1 where nid = 1 or email = 'seven@live.com' and name = 'alex'
- 类型不一致
    如果列是字符串类型，传入条件是必须用引号引起来，不然...
    select * from tb1 where name = 999;
- !=
    select * from tb1 where name != 'alex'
    特别的：如果是主键，则还是会走索引
        select * from tb1 where nid != 123
- >
    select * from tb1 where name > 'alex'
    特别的：如果是主键或索引是整数类型，则还是会走索引
        select * from tb1 where nid > 123
        select * from tb1 where num > 123
- order by
    select email from tb1 order by name desc;
    当根据索引排序时候，选择的映射如果不是索引，则不走索引
    特别的：如果对主键排序，则还是走索引：
        select * from tb1 order by nid desc;
 
- 组合索引最左前缀
    如果组合索引为：(name,email)
    name and email       -- 使用索引
    name                 -- 使用索引
    email                -- 不使用索引
```

###### 6）其他注意事项


```
- 避免使用select *
- count(1)或count(列) 代替 count(*)
- 创建表时尽量时 char 代替 varchar
- 表的字段顺序固定长度的字段优先
- 组合索引代替多个单列索引（经常使用多个条件查询时）
- 尽量使用短索引
- 使用连接（JOIN）来代替子查询(Sub-Queries)
- 连表时注意条件类型需一致
- 索引散列值（重复少）不适合建索引，例：性别不适合
```

###### 7）limit分页

无论是否有索引，limit分页是一个值得关注的问题


```
每页显示10条：
当前 118 120， 125

倒序：
            大      小
            980    970  7 6  6 5  54  43  32

21 19 98     
下一页：

    select 
        * 
    from 
        tb1 
    where 
        nid < (select nid from (select nid from tb1 where nid < 当前页最小值 order by nid desc limit 每页数据 *【页码-当前页】) A order by A.nid asc limit 1)  
    order by 
        nid desc 
    limit 10;



    select 
        * 
    from 
        tb1 
    where 
        nid < (select nid from (select nid from tb1 where nid < 970  order by nid desc limit 40) A order by A.nid asc limit 1)  
    order by 
        nid desc 
    limit 10;


上一页：

    select 
        * 
    from 
        tb1 
    where 
        nid < (select nid from (select nid from tb1 where nid > 当前页最大值 order by nid asc limit 每页数据 *【当前页-页码】) A order by A.nid asc limit 1)  
    order by 
        nid desc 
    limit 10;


    select 
        * 
    from 
        tb1 
    where 
        nid < (select nid from (select nid from tb1 where nid > 980 order by nid asc limit 20) A order by A.nid desc limit 1)  
    order by 
        nid desc 
    limit 10;
```

###### 8）执行计划

explain + 查询SQL - 用于显示SQL执行信息参数，根据参考信息可以进行SQL优化


```
mysql> explain select * from tb2;
+----+-------------+-------+------+---------------+------+---------+------+------+-------+
| id | select_type | table | type | possible_keys | key  | key_len | ref  | rows | Extra |
+----+-------------+-------+------+---------------+------+---------+------+------+-------+
|  1 | SIMPLE      | tb2   | ALL  | NULL          | NULL | NULL    | NULL |    2 | NULL  |
+----+-------------+-------+------+---------------+------+---------+------+------+-------+
1 row in set (0.00 sec)
```


```
    id
        查询顺序标识
            如：mysql> explain select * from (select nid,name from tb1 where nid < 10) as B;
            +----+-------------+------------+-------+---------------+---------+---------+------+------+-------------+
            | id | select_type | table      | type  | possible_keys | key     | key_len | ref  | rows | Extra       |
            +----+-------------+------------+-------+---------------+---------+---------+------+------+-------------+
            |  1 | PRIMARY     | <derived2> | ALL   | NULL          | NULL    | NULL    | NULL |    9 | NULL        |
            |  2 | DERIVED     | tb1        | range | PRIMARY       | PRIMARY | 8       | NULL |    9 | Using where |
            +----+-------------+------------+-------+---------------+---------+---------+------+------+-------------+
        特别的：如果使用union连接气值可能为null


    select_type
        查询类型
            SIMPLE          简单查询
            PRIMARY         最外层查询
            SUBQUERY        映射为子查询
            DERIVED         子查询
            UNION           联合
            UNION RESULT    使用联合的结果
            ...
    table
        正在访问的表名


    type
        查询时的访问方式，性能：all < index < range < index_merge < ref_or_null < ref < eq_ref < system/const
            ALL             全表扫描，对于数据表从头到尾找一遍
                            select * from tb1;
                            特别的：如果有limit限制，则找到之后就不在继续向下扫描
                                   select * from tb1 where email = 'seven@live.com'
                                   select * from tb1 where email = 'seven@live.com' limit 1;
                                   虽然上述两个语句都会进行全表扫描，第二句使用了limit，则找到一个后就不再继续扫描。

            INDEX           全索引扫描，对索引从头到尾找一遍
                            select nid from tb1;

            RANGE          对索引列进行范围查找
                            select *  from tb1 where name < 'alex';
                            PS:
                                between and
                                in
                                >   >=  <   <=  操作
                                注意：!= 和 > 符号


            INDEX_MERGE     合并索引，使用多个单列索引搜索
                            select *  from tb1 where name = 'alex' or nid in (11,22,33);

            REF             根据索引查找一个或多个值
                            select *  from tb1 where name = 'seven';

            EQ_REF          连接时使用primary key 或 unique类型
                            select tb2.nid,tb1.name from tb2 left join tb1 on tb2.nid = tb1.nid;



            CONST           常量
                            表最多有一个匹配行,因为仅有一行,在这行的列值可被优化器剩余部分认为是常数,const表很快,因为它们只读取一次。
                            select nid from tb1 where nid = 2 ;

            SYSTEM          系统
                            表仅有一行(=系统表)。这是const联接类型的一个特例。
                            select * from (select nid from tb1 where nid = 1) as A;
    possible_keys
        可能使用的索引

    key
        真实使用的

    key_len
        MySQL中使用索引字节长度

    rows
        mysql估计为了找到所需的行而要读取的行数 ------ 只是预估值

    extra
        该列包含MySQL解决查询的详细信息
        “Using index”
            此值表示mysql将使用覆盖索引，以避免访问表。不要把覆盖索引和index访问类型弄混了。
        “Using where”
            这意味着mysql服务器将在存储引擎检索行后再进行过滤，许多where条件里涉及索引中的列，当（并且如果）它读取索引时，就能被存储引擎检验，因此不是所有带where子句的查询都会显示“Using where”。有时“Using where”的出现就是一个暗示：查询可受益于不同的索引。
        “Using temporary”
            这意味着mysql在对查询结果排序时会使用一个临时表。
        “Using filesort”
            这意味着mysql会对结果使用一个外部索引排序，而不是按索引次序从表里读取行。mysql有两种文件排序算法，这两种排序方式都可以在内存或者磁盘上完成，explain不会告诉你mysql将使用哪一种文件排序，也不会告诉你排序会在内存里还是磁盘上完成。
        “Range checked for each record(index map: N)”
            这个意味着没有好用的索引，新的索引将在联接的每一行上重新估算，N是显示在possible_keys列中索引的位图，并且是冗余的。
```


###### 9）慢日志查询

a、配置MySQL自动记录慢日志

```
slow_query_log = OFF                            是否开启慢日志记录
long_query_time = 2                              时间限制，超过此时间，则记录
slow_query_log_file = /usr/slow.log        日志文件
log_queries_not_using_indexes = OFF     为使用索引的搜索是否记录
```

注：查看当前配置信息：
　　

```
     show variables like '%query%'
```
修改当前配置：
　　　　

```
set global 变量名 = 值
```

b、查看MySQL慢日志
  
	mysqldumpslow -s at -a  /usr/local/var/mysql/MacBook-Pro-3-slow.log


```
--verbose    版本
--debug      调试
--help       帮助
 
-v           版本
-d           调试模式
-s ORDER     排序方式
             what to sort by (al, at, ar, c, l, r, t), 'at' is default
              al: average lock time
              ar: average rows sent
              at: average query time
               c: count
               l: lock time
               r: rows sent
               t: query time
-r           反转顺序，默认文件倒序拍。reverse the sort order (largest last instead of first)
-t NUM       显示前N条just show the top n queries
-a           不要将SQL中数字转换成N，字符串转换成S。don't abstract all numbers to N and strings to 'S'
-n NUM       abstract numbers with at least n digits within names
-g PATTERN   正则匹配；grep: only consider stmts that include this string
-h HOSTNAME  mysql机器名或者IP；hostname of db server for *-slow.log filename (can be wildcard),
             default is '*', i.e. match all
-i NAME      name of server instance (if using mysql.server startup script)
-l           总时间中不减去锁定时间；don't subtract lock time from total time
```

