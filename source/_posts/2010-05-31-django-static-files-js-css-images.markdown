---
layout: post
title: "Django使用静态文件(JS, CSS, Images)配置"
date: 2010-05-31 15:38
comments: true
categories: [Python, Django, CSS]
---
Django的URL默认是动态访问，对静态文件访问需要进行设置：

*  在 settings.py 文件中定义参数 STATIC_PATH='./static'（意为当前文件目录下的static文件夹）

*  更新urls.py文件:

``` python
import settings
# urlpatterns 添加
(r'^static/(?P<path>.*)$','django.views.static.serve',{'document_root': settings.STATIC_PATH})
```

*  模板中调用
``` python
<link rel="StyleSheet" href="/static/css/base.css" type="text/css" />
```
