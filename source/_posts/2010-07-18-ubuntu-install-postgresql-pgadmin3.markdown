---
layout: post
title: "Ubuntu 安装 PostgreSQL PgAdmin3"
date: 2010-07-18 18:06
comments: true
categories: [Linux, PostgreSQL]
---

### 安装 PostgreSQL
安装 PostgreSQL 和 命令行客户端 psql

``` bash
luo@luo-ubuntu:~$ sudo apt-get install postgresql-8.4 postgresql-client-8.4 postgresql-contrib-8.4
```

### 修改 PostgreSQL 默认用户 postgres 密码
PostgreSQL数据默认会创建一个postgres的用户作为数据库的管理员，密码是随机的，这里需要修改为指定的密码，这里设定为'password':

``` bash
luo@luo-ubuntu:~$ sudo -u postgres psql
postgres=# ALTER USER postgres WITH PASSWORD 'password';
postgres=# \q
```

<!-- more -->

### 修改 Linux 系统 postgres 用户的密码
PostgreSQL 数据默认会创建一个 Linux 用户 postgres，我们要使 posgres 用户与数据库中 postgres 的密码保持一致:

删除密码
``` bash
luo@luo-ubuntu:~$ sudo passwd -d postgres
```

创建密码
``` bash
luo@luo-ubuntu:~$ sudo -u postgres passwd
```

现在就可以用 postgres 账号通过 psql 或者 PgAdmin3 来操作数据库了。

### 修改PostgresSQL数据库配置实现远程访问
修改postgresql.conf的连接权限

``` bash
luo@luo-ubuntu:~$ sudo vi /etc/postgresql/8.4/main/postgresql.conf
将 #listen_addresses = 'localhost' 修改为 listen_addresses = '*'
将 #password_encryption = on       修改为 password_encryption = on
```

修改pg_hba.conf的目的是设置用户操作数据服务器权限
``` bash
luo@luo-ubuntu:~$ sudo vi /etc/postgresql/8.4/main/pg_hba.conf
追加以下2行
# to allow your client visiting postgresql server
host all all 0.0.0.0 0.0.0.0 md5
```

重启PostgreSQL数据库的服务程序，使配置生效
``` bash
luo@luo-ubuntu:~$ sudo /etc/init.d/postgresql-8.4 restart
```

### 安装 PgAdmin3
``` bash
luo@luo-ubuntu:~$ sudo apt-get install pgadmin3
```


### 创建新用户和数据库

``` bash
luo@luo-ubuntu:~$ psql -U postgres -h 127.0.0.1
postgres=# create user 'liluo' with password 'liluo' nocreatedb;
postgres=# create database 'new_database' with owner='liluo';
```

或者 

``` bash
luo@luo-ubuntu:~$ sudo -u postgres createuser -D -P liluo
luo@luo-ubuntu:~$ sudo -u postgres createdb -O liluo new_database
```

__EOF__
