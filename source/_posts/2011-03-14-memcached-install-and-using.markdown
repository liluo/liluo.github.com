---
layout: post
title: "Memcached 安装/使用(Python操作)"
date: 2011-03-14 21:42
comments: true
categories: [Python]
---

Memcached官网 <http://memcached.org>
## 简单介绍
Memcached很强大，它可以支持分布式的共享内存缓存，大型站点都用它。对小站点来说，有足够内存的话，使用它也可以得到超赞的效果。

## 使用目的
由前面的介绍看到，大家使用它都是为了速度，不过我却是为了解决Session在不同浏览器中偶尔丢失的数据。其实也不能怪浏览器啦，主要是我需要一个dict类型的session。

<!-- more -->

## 安装

### Linux

安装
``` bash
# Debian/Ubuntu
sudo apt-get install memcached

# Redhat/Fedora/CentOS
sudo yum install memcached
```

启动Memcached
``` bash
-d 选项是启动一个守护进程
-m 是分配给Memcache使用的内存数量，单位是MB，默认64MB
-M return error on memory exhausted (rather than removing items)
-u 是运行Memcache的用户，如果当前为root 的话，需要使用此参数指定用户
-l 是监听的服务器IP地址，默认为所有网卡
-p 是设置Memcache的TCP监听的端口，最好是1024以上的端口
-c 选项是最大运行的并发连接数，默认是1024
-P 是设置保存Memcache的pid文件
-f chunk size growth factor (default: 1.25)
-I Override the size of each slab page. Adjusts max item size(1.4.2版本新增)
```

例子
``` bash
/usr/local/memcached/bin/memcached -d -m 100 -c 1000 -u root -p 11211
```
可以启动多个守护进程，但是端口不能重复。 设置开机启动的话可以将上行命令增加到/etc/rc.d/rc.local文件中。

### Windows

1.  下载memcache的windows稳定版 <http://splinedancer.com/memcached-win32/>
2.  解压放某个盘下面，比如在c:\memcached
3.  在终端（也即cmd命令界面）cd到解压目录（这里是c:\memcached），运行 memcached.exe -d install 安装服务
4.  运行memcached.exe -d start，memcached会使用默认的端口(11211)来启动，你可以在任务管理器中看到memcached.exe

## Python 操作 Memcached
memcached API地址 http://code.google.com/p/memcached/wiki/Clients

网上流传说Python-API中效率最高的是python-libmemcached，这里居然看到了hongqn（豆瓣首席架构师，后来也得到证实python-libmemcached是豆瓣贡献，看来豆瓣的阳光真的是撒满了Python的各个角落）。

另外还有python-memcached（100%纯Python），python-memcache（据说有内存泄漏问题？），cmemcache（代码多年未更新？）。

不过由于目前需要中效率并没有太高要求，于是选择了使用最多的python-memcached：
安装 python-memcached
``` bash
sudo easy_install python-memcached
```
Python操作memcached
``` python
import memcache
mc = memcache.Client(['127.0.0.1:11211'],debug=True)
mc.set('name','luo',60)
print mc.get('name')
mc.delete('name')
```

## Memcached常用方法
memcache其实是一个map结构，最常用的几个函数：

保存数据
``` bash
set(key,value,timeout) 把key映射到value，timeout指的是什么时候这个映射失效
add(key,value,timeout)  仅当存储空间中不存在键相同的数据时才保存
replace(key,value,timeout) 仅当存储空间中存在键相同的数据时才保存
```

获取数据
``` bash
get(key) 返回key所指向的value
get_multi(key1,key2,key3,key4) 可以非同步地同时取得多个键值， 比循环调用get快数十倍
```

删除数据
``` bash
delete(key, timeout) 删除键为key的数据，timeout为时间值，禁止在timeout时间内名为key的键保存新数据（set函数无效）。
```
