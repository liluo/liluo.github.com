---
layout: post
title: "Linux firewall iptables configuration(防火墙配置)"
date: 2010-08-25 20:25
comments: true
categories: [Linux]
---
之前有讲过公司新买的服务器使用的是CentOS 5.5，部署好Tomcat之后却发现输入114.80.*.*:8080(即ip:8080)却无法显示Tomcat默认的首页。因为以前部署在Win Server的VPS，Linux开发时也只用到localhost，所以就有点头大。

Google大神说这是防火墙问题，关闭防火墙:
``` bash
/etc/init.d/iptables stop
```

再次在浏览器里敲入"114.80.*.*:8080"发现果然成功。这样贸然关闭防火墙是绝对不可取的，正确的做法是在iptables中打开指定端口。了解一下Firewall iptabels：

<!-- more -->

``` bash
/etc/init.d/iptables status   # 查看防火墙当前状态
/etc/init.d/iptables start    # 开启防火墙
/etc/init.d/iptables stop     # 关闭防火墙
/etc/init.d/iptables save     # 保存防火墙规则更新
/etc/init.d/iptables restart  # 重启防火墙
```

打开8080端口
``` bash
/sbin/iptables -I INPUT -p tcp --dport 8080 -j ACCEPT
/etc/init.d/iptables save
```

另外一种打开端口的方式, 在 /etc/sysconfig/iptables 追加
``` bash
-A RH-Firewall-1-INPUT -m state –state NEW -m tcp -p tcp –dport 8080 -j ACCEPT
```

重启防火墙，使新增的配置生效
``` bash
/etc/init.d/iptables restart
```

