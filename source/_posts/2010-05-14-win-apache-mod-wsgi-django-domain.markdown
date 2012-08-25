---
layout: post
title: "windows环境配置apache+mod_wsgi+django+域名(domain)"
date: 2010-05-14 11:53
comments: true
categories: [Python, Django, Domain, WSGI]
---
记录一下Apache+mod_wsgi配置域名跑Django的过程。

系统及软件环境:
> * Windows Xp 
> * Apache(2.2, xampp 1.7.3)
> * Django(1.1.1)
> * mod_wsgi(mod_wsgi-win32-ap22py26-3.0)

<!-- more -->
安装步骤:

-  选择与自己python相匹配的mod_wsgi下载，重命名为mod_wsgi.so并丢到Apache安装目录里的modules 文件夹中。
-  加载mod_wsgi，在Apache配置文件httpd.conf中，增加一行：

``` python
LoadModule wsgi_module modules/mod_wsgi.so
``` 

-  创建命名为demo的项目，并在demo目录中新建conf文件夹。新建conf/demo.wsgi文件，内容如下：

``` python
import os
import sys

sys.stdout = sys.stderr

from os.path import abspath, dirname, join
from site import addsitedir
from django.core.handlers.wsgi import WSGIHandler

sys.path.insert(0, abspath(join(dirname(__file__), "../")))
sys.path.insert(0, abspath(join(dirname(__file__), "../../")))
os.environ["DJANGO_SETTINGS_MODULE"] = "demo.settings " #your settings module

application = WSGIHandler()
```

-  配置域名
   1.   在 C:\WINDOWS\system32\drivers\etc\hosts文件中添加"127.0.0.1 liluo.com" (将liluo.com替换成自己的域名)
   2.   找到apache\conf\extra目录httpd-vhosts.conf文件中"#NameVirtualHost *:80" 这行，将"#"去掉。在该文件内追加以下内容：

```
<VirtualHost *:80>
    ServerName      liluo.com        #测试域名
    ServerAlias       www.liluo.com    #测试域名
    DocumentRoot    F:/htdocs
    WSGIScriptAlias / F:/htdocs/demo/conf/demo.wsgi
    Alias /static F:/htdocs/demo/static

    <Location "/static">
        SetHandler None
    </Location>

    <Directory " F:/htdocs/demo/static">
        Order Deny,Allow
        Allow from all
    </Directory>

    <Directory " F:/htdocs/demo/wsgi">
        Order Deny,Allow
        Allow from all
    </Directory>

    <Directory "/usr/local/lib/site-packages/django/contrib/admin/media">
        Order Deny,Allow
        Allow from all
    </Directory>

    Alias "/media"  "/usr/local/lib/site-packages/django/contrib/admin/media"
    <Location "/media">
        SetHandler None
    </Location>
</VirtualHost>

```

-  重启Apache, 即可看到“It worked!”
