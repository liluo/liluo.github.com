---
layout: post
title: "将 CentOS 5.5 中 Python 更新到2.6.5"
date: 2010-08-19 19:40
comments: true
categories: [Linux, Python]
---

前天公司购买一台服务器(放置在外高桥电信机房，跑的是JSP的应用)，不想在服务器上使用盗版的Win server(当然也是为了公司节省软件许可费用)，于是安装了 CentOS 5.5(貌似是目前比较新的版本？)。BTW 它的 Python 居然是2.4.3的版本，阿门。

于是的于是就有了下面给Python升级的过程(CentOS 5.5 中实验成功，其他发行版本Linux可作参考)。

<!-- more -->

下载
``` bash
wget http://www.python.org/ftp/python/2.6.5/Python-2.6.5.tar.bz2
```

解压
``` bash
tar jxvf Python-2.6.5.tar.bz2
```

编译安装
``` bash
cd Python-2.6.5
./configure
make && make install
```

检查
``` bash
/usr/local/bin/python2.6 -V # Python 2.6.5
```

默认使用Python新版本
``` bash
mv /usr/bin/python /usr/bin/python.bak
ln -s /usr/local/bin/python2.6 /usr/bin/python
python -V # Python 2.6.5
```

### 修复不 work 的yum

完成以上几步之后，如果使用 yum 命令的话会报错：
``` bash
no module named yum
```
这是因为 yum 依赖 Python 2.4.3 而现在默认的 Python 版本是2.6.5。修复也很简单，将 /usr/bin/yum 文件的首行
``` bash
!#/usr/bin/python
```
修改为
``` bash
!#/usr/bin/python2.4
```

