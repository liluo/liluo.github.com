---
layout: post
title: "Fedora 配置 Nginx + MySQL PostgreSQL Python PHP(FastCGI)"
date: 2010-06-04 15:59
comments: true
categories: [Linux, Nginx, PostgreSQL, MySQL, Python]
---
记录一下安装配置 Linux+Nginx+MySQL+PostgreSQL+Python+PHP 的过程。就是传说中的 LNMPPP～说白了就是 OS 使用 Linux, Web server 使用 Nginx, 支持Python和PHP，数据库支持PostgreSQL和MySQL。开始吧～

<!-- more -->

#### Linux
```
Fedora 13(其他Linux发行版可能需要少许变动)
```

#### Nginx
安装
``` bash
yum install nginx
```

添加到系统自动运行
``` bash
chkconfig --levels 235 nginx on
```

启动
``` bash
/etc/init.d/nginx start
```
Nginx 已经安装并启动，访问下 http://localhost/ 试一下吧

#### MySQL
安装
``` bash
yum install mysql mysql-server
```

添加系统服务并启动
``` bash
chkconfig --levels 235 mysqld on
```

启动
``` bash
/etc/init.d/mysqld start
```

检查是否支持网络连接
``` bash
netstat -tap | grep mysql
```

应该会看到这样的状态
``` bash
netstat -tap | grep mysql
tcp        0      0 *:mysql       *:*              LISTEN      1376/mysqld
```

如果不是的话，需要修改/etc/my.cnf文件来启用网络连接支持, 将文件中 "#skip-networking" 的 "#" 去掉

重启mysql
``` bash
/etc/init.d/mysqld restart
```

MySQL默认root用户的密码为空，需要给root设置密码(* 代表密码字符)
``` bash
mysqladmin -u root password *****
```

#### PostgreSQL
安装
``` bash
yum install postgresql postgresql-server
```

然后需要初始化
``` bash
Service postgresql initdb
```

打开/var/lib/pgsql/data/pg_hba.conf文件，将Ipv4 local connections一栏中数据改为：
``` bash
host    all         all         0.0.0.0           trust
```

添加远程连接，将/var/lib/pgsql/data/postgresql.conf中的listen_sddress和port的注释删除，并改为：
``` bash
listen_address = '*'
port = 5432
```

[可选]安装postgresql管理工具pgadmin3
``` bash
yum install pgadmin3
```

[可选]添加对python的支持
``` bash
yum install python-psycopg2
```

#### Python
呃，其实 Fedora 自带的 Python 版本已经是比较新的了，只需要安装flup配置一下就ok了。

安装flup
``` bash
sudo easy_install flup
```

更新 Nginx 配置文件 nginx.conf
``` bash
server {
    listen 80;
    server_name localhost;

    # site_media - folder in uri for static files
    location /static  {
        root /var/www/static;
    }

    location ~* ^.+\.(jpg|jpeg|gif|png|ico|css|zip|tgz|gz|rar|bz2|doc|xls|exe|pdf|ppt|txt|tar|mid|midi|wav|bmp|rtf|js|mov) {
        access_log   off; 
        expires      30d;
    }
    location / {
        # host and port to fastcgi server
        fastcgi_pass 127.0.0.1:8000;
        fastcgi_param PATH_INFO      $fastcgi_script_name;
        fastcgi_param REQUEST_METHOD $request_method;
        fastcgi_param QUERY_STRING   $query_string;
        fastcgi_param CONTENT_TYPE   $content_type;
        fastcgi_param CONTENT_LENGTH $content_length;
        fastcgi_pass_header          Authorization;
        fastcgi_intercept_errors     off;
    }
    
   access_log /var/log/nginx/localhost.access_log main;
   error_log  /var/log/nginx/localhost.error_log;
}

```

#### PHP
安装 PHP
``` bash
yum install php-cli php-mysql php-pgsql php-gd php-imap php-ldap php-odbc php-pear php-xml php-xmlrpc php-eaccelerator php-magickwand php-magpierss php-mapserver php-mbstring php-mcrypt php-shout php-snmp php-soap php-tidy
```
在 /etc/php.ini 文件中追加'cgi.fix_pathinfo=1'

更新 Nginx 配置文件 nginx.conf
``` bash
server {
    listen       80;
    server_name  localhost;

    #charset koi8-r;
    #access_log  logs/host.access.log  main;

    location / {
        root   /usr/share/nginx/html;
        index  index.php index.html index.htm;
    }

    error_page  404              /404.html;
    location = /404.html {
        root   /usr/share/nginx/html;
    }
    error_page 500 502 503 504  /50x.html;
    location = /50x.html {
        root   /var/www;
    }

    location ~ \.php$ {
        root           html;
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  /var/www$fastcgi_script_name;
        include        fastcgi_params;
    }
    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    location ~ /\.ht {
        deny  all;
    }
}
```

以 FCGI 方式在9000端口启动 PHP
``` bash
/usr/bin/spawn-fcgi -a 127.0.0.1 -p 9000 -u nginx -g nginx -f /usr/bin/php-cgi -P /var/run/fastcgi-php.pid
```
每次手动启动很麻烦，可以直接把上面内容追加到 /etc/rc.local 文件实现自动启动。

__EOF__
