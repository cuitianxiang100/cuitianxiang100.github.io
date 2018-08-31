---
layout:     post
title:      "Python笔记day43（MySQL）|增删改查、通配符、分页、分组、排序、连表"
subtitle:   "增删改查、通配符、分页、分组、排序、连表"
date:       2018-08-31 19:31:00
author:     "崔天祥"
header-img: "img/post-bg-os-metro.jpg"
catalog:    true
tags:
    - Python
    - 编程
    - MySQL
---

> 增删改查、通配符、分页、分组、排序、连表 | 若本文不能正常显示 [点击这里](https://blog.csdn.net/qq_34755081/article/details/82260833)
#### 1，内容回顾

```
补充：主键
					一个表只能有一个主键
					主键可以由多列组成
					
					
				补充：外键 ?
					CREATE TABLE t5 (
					  nid int(11) NOT NULL AUTO_INCREMENT,
					  pid int(11) not NULL,
					  num int(11),
					  primary key(nid,pid)
					) ENGINE=InnoDB DEFAULT CHARSET=utf8;



					create table t6(
						id int auto_increment primary key,
						name char(10),
						id1 int,
						id2 int,
						CONSTRAINT fk_t5_t6 foreign key (id1,id2) REFERENCES t1(nid,pid)
					)engine=innodb default charset=utf8;
```


----------


#### 2，sql操作
###### 1）自增
<!-- more -->
```
对于自增补充：
	
		
		show create table t10;
		
		show create table t10 \G;
		
		alter table t10 AUTO_INCREMENT=20;
			
			
		MySQL: 自增步长
			基于会话级别（退出当前登录后步长设置失效）：
				show session variables like 'auto_inc%';	查看全局变量
                set session auto_increment_increment=2; 	设置会话步长
				# set session auto_increment_offset=10;
			基于全局级别：
				show global variables like 'auto_inc%';	    查看全局变量
                set global auto_increment_increment=2; 	    设置会话步长
				# set global auto_increment_offset=10;
```
###### 2）唯一索引

```
create table t1(
			id int ....,
			num int,
			xx int,
			unique 唯一索引名称 (列名,列名),
			constraint ....
		)
		# 
		1   1   1
		2   1   2
		PS: 
			唯一：
				约束不能重复（可以为空）
				PS: 主键不能重复（不能为空）
			加速查找
```
###### 3）一对一

```
create table userinfo1(
					id int auto_increment primary key,
					name char(10),
					gender char(10),
					email varchar(64)
				)engine=innodb default charset=utf8;

				create table admin(
					id int not null auto_increment primary key,
					username varchar(64) not null,
					password VARCHAR(64) not null,
					user_id int not null,
					unique uq_u1 (user_id),
					CONSTRAINT fk_admin_u1 FOREIGN key (user_id) REFERENCES userinfo1(id)
				)engine=innodb default charset=utf8;

```
###### 4）多对多

```
reate table userinfo2(
					id int auto_increment primary key,
					name char(10),
					gender char(10),
					email varchar(64)
				)engine=innodb default charset=utf8;

				create table host(
					id int auto_increment primary key,
					hostname char(64)
				)engine=innodb default charset=utf8;


				create table user2host(
					id int auto_increment primary key,
					userid int not null,
					hostid int not null,
					unique uq_user_host (userid,hostid),
					CONSTRAINT fk_u2h_user FOREIGN key (userid) REFERENCES userinfo2(id),
					CONSTRAINT fk_u2h_host FOREIGN key (hostid) REFERENCES host(id)
				)engine=innodb default charset=utf8;
```
#### 3， SQL语句数据行操作补充

```
create table tb12(
				id int auto_increment primary key,
				name varchar(32),
				age int
			)engine=innodb default charset=utf8;
	
```
###### 增
			

```
insert into tb11(name,age) values('alex',12);
			
			insert into tb11(name,age) values('alex',12),('root',18);
			
			insert into tb12(name,age) select name,age from tb11;
```
###### 删
			

```
delete from tb12;
			delete from tb12 where id !=2 
			delete from tb12 where id =2 
			delete from tb12 where id > 2 
			delete from tb12 where id >=2 
			delete from tb12 where id >=2 or name='alex'
```
		
###### 改
			

```
update tb12 set name='alex' where id>12 and name='xx'
			update tb12 set name='alex',age=19 where id>12 and name='xx'
```
###### 查
			
			

```
select * from tb12;
			
			select id,name from tb12;
			
			select id,name from tb12 where id > 10 or name ='xxx';
			
			select id,name as cname from tb12 where id > 10 or name ='xxx';
			
			select name,age,11 from tb12;
```
			
###### 其他：
			

```
				select * from tb12 where id != 1
				select * from tb12 where id in (1,5,12);
				select * from tb12 where id not in (1,5,12);
				select * from tb12 where id in (select id from tb11)
				select * from tb12 where id between 5 and 12;
```
	
			
###### 通配符：
				
				

```
                select * from tb12 where name like "a%"
				select * from tb12 where name like "a_"
```
	
			
###### 分页：

```
				
					select * from tb12 limit 10;
					
					select * from tb12 limit 0,10;
					select * from tb12 limit 10,10;
					select * from tb12 limit 20,10;
					
					select * from tb12 limit 10 offset 20;
					从第20行开始读取，读取10行；
		
					结合Python分页：
					# page = input('请输入要查看的页码')
					# page = int(page)
					# (page-1) * 10
					# select * from tb12 limit 0,10; 1 
					# select * from tb12 limit 10,10;2
				
				
```
###### 排序：

```
					select * from tb12 order by id desc; 大到小
					select * from tb12 order by id asc;  小到大
					 select * from tb12 order by age desc,id desc;
					 
					取后10条数据
					select * from tb12 order by id desc limit 10;
			
```
###### 分组：
				
```
					select count(id),max(id),part_id from userinfo5 group by part_id;
					
					count
					max
					min
					sum
					avg
					
					**** 如果对于聚合函数结果进行二次筛选时？必须使用having ****
					select count(id),part_id from userinfo5 group by part_id having count(id) > 1;
					
					select count(id),part_id from userinfo5 where id > 0 group by part_id having count(id) > 1;
```
			
					
###### 连表操作：
				
```
					select * from userinfo5,department5
					
					select * from userinfo5,department5 where userinfo5.part_id = department5.id
					

					select * from userinfo5 left join department5 on userinfo5.part_id = department5.id
					select * from department5 left join userinfo5 on userinfo5.part_id = department5.id
					# userinfo5左边全部显示
					
					
					# select * from userinfo5 right join department5 on userinfo5.part_id = department5.id
					# department5右边全部显示
				
				
				
					select * from userinfo5 innder join department5 on userinfo5.part_id = department5.id
					将出现null时一行隐藏
					
					
				
				
				
				
					select * from 
						department5 
					left join userinfo5 on userinfo5.part_id = department5.id
					left join userinfo6 on userinfo5.part_id = department5.id
				
				
					select 
						score.sid,
						student.sid 
						from 
					score

						left join student on score.student_id = student.sid

						left join course on score.course_id = course.cid

						left join class on student.class_id = class.cid

						left join teacher on course.teacher_id=teacher.tid
					
			
			
			
			select count(id) from userinfo5;
```