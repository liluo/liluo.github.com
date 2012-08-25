---
layout: post
title: "运行在Apache中Django后台admin界面CSS丢失"
date: 2010-05-19 15:27
comments: true
categories: [Python, Django, Apache, CSS]
---
在 Apache 的 conf/extra/httpd-vhosts.conf 加入以下代码:
```
<VirtualHost *:80>

    <Directory "/usr/local/lib/site-packages/django/contrib/admin/media">
        Order Deny,Allow
        Allow from all
    </Directory>  
 
    Alias "/media" "/usr/local/lib/site-packages/django/contrib/admin/media"
    <Location "/media">
        SetHandler None
    </Location>

</VirtualHost>
```
