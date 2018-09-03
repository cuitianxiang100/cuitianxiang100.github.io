---
layout:     post
title:      "Python笔记day46-2（MySQL）|SQLAchemy模块、relationship用法"
subtitle:   "SQLAchemy模块、relationship用法"
date:       2018-09-02 21:47:00
author:     "崔天祥"
header-img: "img/post-bg-os-metro.jpg"
catalog:    true
tags:
    - Python
    - 编程
    - MySQL
---

> SQLAchemy模块、relationship用法 | 若本文不能正常显示 [点击这里](https://blog.csdn.net/qq_34755081/article/details/82319894)

#### SQLAchemy

SQLAlchemy是Python编程语言下的一款ORM框架，该框架建立在数据库API之上，使用关系对象映射进行数据库操作，简言之便是：将对象转换成SQL，然后使用数据API执行SQL并获取执行结果。

SQLAlchemy本身无法操作数据库，其必须以来pymsql等第三方插件，Dialect用于和数据API进行交流，根据配置文件的不同调用不同的数据库API，从而实现对数据库的操作，如：

```
MySQL-Python
    mysql+mysqldb://<user>:<password>@<host>[:<port>]/<dbname>
   
pymysql
    mysql+pymysql://<username>:<password>@<host>/<dbname>[?<options>]
   
MySQL-Connector
    mysql+mysqlconnector://<user>:<password>@<host>[:<port>]/<dbname>
   
cx_Oracle
    oracle+cx_oracle://user:pass@host:port/dbname[?key=value&key=value...]
```

<!-- more -->
#### 1，内部处理

使用 Engine/ConnectionPooling/Dialect 进行数据库操作，Engine使用ConnectionPooling连接数据库，然后再通过Dialect执行SQL语句。


```
#!/usr/bin/env python
# -*- coding:utf-8 -*-
from sqlalchemy import create_engine
  
  
engine = create_engine("mysql+pymysql://root:123@127.0.0.1:3306/t1", max_overflow=5)
  
# 执行SQL
# cur = engine.execute(
#     "INSERT INTO hosts (host, color_id) VALUES ('1.1.1.22', 3)"
# )
  
# 新插入行自增ID
# cur.lastrowid
  
# 执行SQL
# cur = engine.execute(
#     "INSERT INTO hosts (host, color_id) VALUES(%s, %s)",[('1.1.1.22', 3),('1.1.1.221', 3),]
# )
  
  
# 执行SQL
# cur = engine.execute(
#     "INSERT INTO hosts (host, color_id) VALUES (%(host)s, %(color_id)s)",
#     host='1.1.1.99', color_id=3
# )
  
# 执行SQL
# cur = engine.execute('select * from hosts')
# 获取第一行数据
# cur.fetchone()
# 获取第n行数据
# cur.fetchmany(3)
# 获取所有数据
# cur.fetchall()
```


----------


#### 2，ORM功能使用

使用 ORM/Schema Type/SQL Expression Language/Engine/ConnectionPooling/Dialect 所有组件对数据进行操作。根据类创建对象，对象转换成SQL，执行SQL。

###### 1）创建表


```
#!/usr/bin/env python
# -*- coding:utf-8 -*-
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy import Column, Integer, String, ForeignKey, UniqueConstraint, Index
from sqlalchemy.orm import sessionmaker, relationship
from sqlalchemy import create_engine
 
engine = create_engine("mysql+pymysql://root:123@127.0.0.1:3306/t1", max_overflow=5)
 
Base = declarative_base()
 
# 创建单表
class Users(Base):
    __tablename__ = 'users'
    id = Column(Integer, primary_key=True)
    name = Column(String(32))
    extra = Column(String(16))
 
    __table_args__ = (
    UniqueConstraint('id', 'name', name='uix_id_name'),
        Index('ix_id_name', 'name', 'extra'),
    )
 
 
# 一对多
class Favor(Base):
    __tablename__ = 'favor'
    nid = Column(Integer, primary_key=True)
    caption = Column(String(50), default='red', unique=True)
 
 
class Person(Base):
    __tablename__ = 'person'
    nid = Column(Integer, primary_key=True)
    name = Column(String(32), index=True, nullable=True)
    favor_id = Column(Integer, ForeignKey("favor.nid"))
 
 
# 多对多
class Group(Base):
    __tablename__ = 'group'
    id = Column(Integer, primary_key=True)
    name = Column(String(64), unique=True, nullable=False)
    port = Column(Integer, default=22)
 
 
class Server(Base):
    __tablename__ = 'server'
 
    id = Column(Integer, primary_key=True, autoincrement=True)
    hostname = Column(String(64), unique=True, nullable=False)
 
 
class ServerToGroup(Base):
    __tablename__ = 'servertogroup'
    nid = Column(Integer, primary_key=True, autoincrement=True)
    server_id = Column(Integer, ForeignKey('server.id'))
    group_id = Column(Integer, ForeignKey('group.id'))
 
 
def init_db():
    Base.metadata.create_all(engine)
 
 
def drop_db():
    Base.metadata.drop_all(engine)
```

注：设置外检的另一种方式 ForeignKeyConstraint(['other_id'], ['othertable.other_id'])

###### 2）操作表

```
#!/usr/bin/env python
# -*- coding:utf-8 -*-
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy import Column, Integer, String, ForeignKey, UniqueConstraint, Index
from sqlalchemy.orm import sessionmaker, relationship
from sqlalchemy import create_engine

engine = create_engine("mysql+pymysql://root:123@127.0.0.1:3306/t1", max_overflow=5)

Base = declarative_base()

# 创建单表
class Users(Base):
    __tablename__ = 'users'
    id = Column(Integer, primary_key=True)
    name = Column(String(32))
    extra = Column(String(16))

    __table_args__ = (
    UniqueConstraint('id', 'name', name='uix_id_name'),
        Index('ix_id_name', 'name', 'extra'),
    )

    def __repr__(self):
        return "%s-%s" %(self.id, self.name)

# 一对多
class Favor(Base):
    __tablename__ = 'favor'
    nid = Column(Integer, primary_key=True)
    caption = Column(String(50), default='red', unique=True)

    def __repr__(self):
        return "%s-%s" %(self.nid, self.caption)

class Person(Base):
    __tablename__ = 'person'
    nid = Column(Integer, primary_key=True)
    name = Column(String(32), index=True, nullable=True)
    favor_id = Column(Integer, ForeignKey("favor.nid"))
    # 与生成表结构无关，仅用于查询方便
    favor = relationship("Favor", backref='pers')

# 多对多
class ServerToGroup(Base):
    __tablename__ = 'servertogroup'
    nid = Column(Integer, primary_key=True, autoincrement=True)
    server_id = Column(Integer, ForeignKey('server.id'))
    group_id = Column(Integer, ForeignKey('group.id'))
    group = relationship("Group", backref='s2g')
    server = relationship("Server", backref='s2g')

class Group(Base):
    __tablename__ = 'group'
    id = Column(Integer, primary_key=True)
    name = Column(String(64), unique=True, nullable=False)
    port = Column(Integer, default=22)
    # group = relationship('Group',secondary=ServerToGroup,backref='host_list')


class Server(Base):
    __tablename__ = 'server'

    id = Column(Integer, primary_key=True, autoincrement=True)
    hostname = Column(String(64), unique=True, nullable=False)




def init_db():
    Base.metadata.create_all(engine)


def drop_db():
    Base.metadata.drop_all(engine)


Session = sessionmaker(bind=engine)
session = Session()
```

###### 增


```
obj = Users(name="alex0", extra='sb')
session.add(obj)
session.add_all([
    Users(name="alex1", extra='sb'),
    Users(name="alex2", extra='sb'),
])
session.commit()
```

###### 删

```
session.query(Users).filter(Users.id > 2).delete()
session.commit()
```

###### 改

```
session.query(Users).filter(Users.id > 2).update({"name" : "099"})
session.query(Users).filter(Users.id > 2).update({Users.name: Users.name + "099"}, synchronize_session=False)
session.query(Users).filter(Users.id > 2).update({"num": Users.num + 1}, synchronize_session="evaluate")
session.commit()
```

###### 查


```
ret = session.query(Users).all()
ret = session.query(Users.name, Users.extra).all()
ret = session.query(Users).filter_by(name='alex').all()
ret = session.query(Users).filter_by(name='alex').first()

ret = session.query(Users).filter(text("id<:value and name=:name")).params(value=224, name='fred').order_by(User.id).all()

ret = session.query(Users).from_statement(text("SELECT * FROM users where name=:name")).params(name='ed').all()
```


###### 其他


```
#　条件
ret = session.query(Users).filter_by(name='alex').all()
ret = session.query(Users).filter(Users.id > 1, Users.name == 'eric').all()
ret = session.query(Users).filter(Users.id.between(1, 3), Users.name == 'eric').all()
ret = session.query(Users).filter(Users.id.in_([1,3,4])).all()
ret = session.query(Users).filter(~Users.id.in_([1,3,4])).all()
ret = session.query(Users).filter(Users.id.in_(session.query(Users.id).filter_by(name='eric'))).all()
from sqlalchemy import and_, or_
ret = session.query(Users).filter(and_(Users.id > 3, Users.name == 'eric')).all()
ret = session.query(Users).filter(or_(Users.id < 2, Users.name == 'eric')).all()
ret = session.query(Users).filter(
    or_(
        Users.id < 2,
        and_(Users.name == 'eric', Users.id > 3),
        Users.extra != ""
    )).all()


# 通配符
ret = session.query(Users).filter(Users.name.like('e%')).all()
ret = session.query(Users).filter(~Users.name.like('e%')).all()

# 限制
ret = session.query(Users)[1:2]

# 排序
ret = session.query(Users).order_by(Users.name.desc()).all()
ret = session.query(Users).order_by(Users.name.desc(), Users.id.asc()).all()

# 分组
from sqlalchemy.sql import func

ret = session.query(Users).group_by(Users.extra).all()
ret = session.query(
    func.max(Users.id),
    func.sum(Users.id),
    func.min(Users.id)).group_by(Users.name).all()

ret = session.query(
    func.max(Users.id),
    func.sum(Users.id),
    func.min(Users.id)).group_by(Users.name).having(func.min(Users.id) >2).all()

# 连表

ret = session.query(Users, Favor).filter(Users.id == Favor.nid).all()

ret = session.query(Person).join(Favor).all()

ret = session.query(Person).join(Favor, isouter=True).all()


# 组合
q1 = session.query(Users.name).filter(Users.id > 2)
q2 = session.query(Favor.caption).filter(Favor.nid < 2)
ret = q1.union(q2).all()

q1 = session.query(Users.name).filter(Users.id > 2)
q2 = session.query(Favor.caption).filter(Favor.nid < 2)
ret = q1.union_all(q2).all()
```


----------

#### 3，relationship用法

```
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy import Column, Integer, String, ForeignKey, UniqueConstraint, Index,CHAR,VARCHAR
from sqlalchemy.orm import sessionmaker, relationship
from sqlalchemy import create_engine

Base = declarative_base()


# 创建单表
"""
1   白金
2   黑金
obj.xx ==> [obj,obj...]
"""
class UserType(Base):
    __tablename__ = 'usertype'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(VARCHAR(32), nullable=True, index=True)

"""
1   方少伟   1
2   成套     1
3   小白     2
# 正向
ut = relationship（backref='xx'）
obj.ut ==> 1   白金
"""
class Users(Base):
    __tablename__ = 'users'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(VARCHAR(32), nullable=True, index=True)
    email = Column(VARCHAR(16), unique=True)
    user_type_id = Column(Integer,ForeignKey("usertype.id"))

    user_type = relationship("UserType",backref='xxoo')
   
```

```
# 问题1. 获取用户信息以及与其关联的用户类型名称(FK,Relationship=>正向操作)
# user_list = session.query(Users,UserType).join(UserType,isouter=True)
# print(user_list)
# for row in user_list:
#     print(row[0].id,row[0].name,row[0].email,row[0].user_type_id,row[1].title)

# user_list = session.query(Users.name,UserType.title).join(UserType,isouter=True).all()
# for row in user_list:
#     print(row[0],row[1],row.name,row.title)


# user_list = session.query(Users)
# for row in user_list:
#     print(row.name,row.id,row.user_type.title)


# 问题2. 获取用户类型
# type_list = session.query(UserType)
# for row in type_list:
#     print(row.id,row.title,session.query(Users).filter(Users.user_type_id == row.id).all())

# type_list = session.query(UserType)
# for row in type_list:
#     print(row.id,row.title,row.xxoo)

```