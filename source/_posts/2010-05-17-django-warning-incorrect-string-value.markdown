---
layout: post
title: "django 保存中文抛出 warning incorrect string value"
date: 2010-05-17 12:39
comments: true
categories: [Python, Django] 
---

Django的ORM对象存储中文的时抛出
``` python
Waring: Incorrect string value:'\xc2\xe4\xc2\xe4" for ...
```
### 中文乱码或出错原因
Django默认使用UTF-8字符，mysql默认使用litan1字符集。我们需要修改my.ini(my.conf)，配置default-charset，然后重建数据库。

<!-- more -->

### 解决方法
-  修改mysql/bin/my.ini(有的版本是mysql/my.cnf)
```
# 在[client] 下面添加
default-character-set=utf8

# 在[mysqld] 下面添加
default-character-set=utf8
init_connect='SET NAMES utf8'

# 在[mysql] 下面添加
default-character-set=utf8

```
-  删除已有的数据表，然后重新创建使用"manage.py syncdb"创建表(如果数据不多的话，这里推荐直接删除数据库，然后重新建同名数据库)。

