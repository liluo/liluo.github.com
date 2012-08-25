---
layout: post
title: "Ubuntu 中 MySQL 卸载、重装以及编码问题"
date: 2010-06-30 17:49
comments: true
categories: [MySQL] 
---
最近Ubuntu中的MySQL出了点问题，网上找了N久也没找到答案，于是华丽的将它卸载重装。当然，如标题所写，这里还有涉及到编码问题。

<!-- more -->

### 卸载

当初安装时用的是:
``` bash
sudo apt-get install mysql-server mysql-client
```

于是相应的卸载应该是:
``` bash
sudo aptitude purge mysql-server mysql-client
```
>  其实我也有尝试用新立得软件管理器来卸载，再次安装的时候却被跳过设置root密码的步骤，应该是卸载的不完全吧？但是不管怎样我用上面的命令卸载之后重新安装正常。

### 安装 MySQL
``` bash
sudo apt-get install mysql-server mysql-client
```

是的，依然是原来安装的方式，需要输入root的密码并确认一次。但是当我安装完成之后发现无法使用sudo /etc/init.d/mysql restart来重启以及启动、停止，出现以下提示：

``` bash
luo@luo-ubuntu:~$ sudo /etc/init.d/mysql restart
Rather than invoking init scripts through /etc/init.d, use the service(8)
utility, e.g. service mysql restart
Since the script you are attempting to invoke has been converted to an
Upstart job, you may also use the restart(8) utility, e.g. restart mysql
mysql start/running, process 3942
luo@luo-ubuntu:~$ sudo /etc/init.d/mysql restartRather than invoking init scripts through /etc/init.d, use the service(8)utility, e.g. service mysql restart
Since the script you are attempting to invoke has been converted to anUpstart job, you may also use the restart(8) utility, e.g. restart mysqlmysql start/running, process 3942
```

于是参照提示使用以下命令行来重启、启动、停止Mysql：
``` bash
sudo restart mysql
sudo start mysql
sudo stop mysql
```

### 编码
网上有流传说Mysql的默认编码是latin1，我之前的Mysql也是，但是当我重装完成之后进入Mysql使用：show variables like "character_set%"; ，結果如下：

``` bash
mysql> show variables like 'character%';
+--------------------------+----------------------------+
| Variable_name | Value |
+--------------------------+----------------------------+
| character_set_client | utf8 |
| character_set_connection | utf8 |
| character_set_database | utf8 |
| character_set_filesystem | binary |
| character_set_results | utf8 |
| character_set_server | utf8 |
| character_set_system | utf8 |
| character_sets_dir | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.00 sec)
```

这也是新版本带来的吗？我不能确定，所以我还是把修改Mysql编码的方法记录一下。

首先，进入Mysql使用 show variables like 'character%';查看，如果执行编码显示:
``` bash
+--------------------------+----------------------------+
| Variable_name | Value |
+--------------------------+----------------------------+
| character_set_client | latin1 |
| character_set_connection | latin1 |
| character_set_database | latin1 |
| character_set_filesystem | binary |
| character_set_results | latin1 |
| character_set_server | latin1 |
| character_set_system | utf8 |
| character_sets_dir | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
```

说明需要修改编码，修改/etc/mysql/my.cnf文件:

找到客户端配置[client] 在下面添加
``` bash
default-character-set=utf8 默认字符集为utf8
```

在找到[mysqld] 添加
``` bash
default-character-set=utf8
init_connect='SET NAMES utf8'
```
修改好后，重新启动mysql 即可，查询一下show variables like 'character%'。

如果不出意外，已经Ok。
