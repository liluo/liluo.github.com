---
layout: post
title: "Django ImportError: Settings cannot be imported"
date: 2010-04-06 17:30
comments: true
categories: [Python, Django]
---
使用Django时在命令行中敲击Python命令进入交互模式，如果直接如下操作：

``` python
from django.template import  Template ,Context
t  = Template("Test is {{test}}")
```

会导致错误: 

``` python
ImportError: Settings cannot be imported, because environment variable DJANGO_SETTINGS_MODULE is undefined.
```
<!-- more -->

原因是django的配置信息没有初始化。解决方法有两种：

1. 切换到Project或者APP所在的目录使用manage.py shell（或者python manage.py shell）命令启动交互窗口;
2. 手动将django的配置初始化：

``` python
from django.conf import settings
settings.configure()
```
