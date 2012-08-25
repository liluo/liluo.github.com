---
layout: post
title: "Linux 安装 MongoDB及简单入门"
date: 2011-03-29 22:02
comments: true
categories: [Linux, MongoDB] 
---

MongoDB是一个使用由C++编写的基于分布式文件存储的数据库开源项目，旨在为WEB应用提供可护展的高性能数据存储解决方案。

下面说下安装方法以及简单入门知识。

<!-- more -->

## 下载
到官网 <http://mongodb.org> 去下载最新的稳定版本，目前是 mongodb-linux-i686-1.8.0.tgz

## 解压
``` bash
mv mongodb-linux-i686-1.8.0.tgz /usr/local/
cd /usr/local
tar xvf mongodb-linux-i686-1.8.0.tgz
mv mongodb-linux-i686-1.8.0 mondodb
rm -y mongodb-linux-i686-1.8.0.tgz
```

## 运行
需要创建一个存放数据的目录(默认是/data/db/)，创建目录并启动：
``` bash
mkdir -p /data/db/
/usr/local/mongodb/bin/mongod
```
如果想使用自己指定的目录来存储数据，加上--dbpath选项：
``` bash
/usr/local/mongodb/bin/mongod --dbpath /path/to/data/dir
```

常用参数说明
``` bash
--port: 指定端口，默认 27017
--dbpath: 指定数据目录，默认 /data/db
--logpath: 指定日志如初路径，如果不指定的话，则将日志输出到命令行。
--logappend: 创建日志时，会将原有文件覆盖，使用这个选项可以追加写日志。
--fork: 以守护进程的方式运行MongoDB
--rest: 启用MongoDB REST API，可以用默认端口 +1000 来管理数据库。
--config: 指定配置文件
```

## 使用JavaScript Shell工具
默认链接的是test数据库
``` bash
/usr/local/mongodb/bin/mongo
MongoDB shell version: 1.8.0
connecting to: test
>
```

## MongoDB 手册
<http://www.mongodb.org/display/DOCS/Manual>
