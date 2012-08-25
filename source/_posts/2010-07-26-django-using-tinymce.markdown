---
layout: post
title: "Django 使用 TinyMCE 编辑器(包括admin后台)"
date: 2010-07-26 18:41
comments: true
categories: 
---
其实无论是否Django开发的项目，我们往往需要添加一个在线编辑器。

这里来简单的介绍下Django中使用TinyMCE在线编辑器的方法，在其他程序中使用或者使用其他编辑器也可作参考。

<!-- more -->

-  下载TinyMCE, 到 http://tinymce.moxiecode.com/ 官方主页去下载最新版本；

-  将下载到的压缩包解压并放在指定目录(这里是 Django 项目的 static 目录)；

-  在 tiny_mce 目录新建 textareas.js 文件，内容参考 <http://tinymce.moxiecode.com/examples/full.php>，仅需用到第3~33行即可；

-  配置 Django 静态资源的访问，可参考 [Django使用静态文件(JS, CSS, Images)配置](/blog/2010/05/django-static-files-js-css-images/)

-  在模板页面<head></head>之间加入
``` html
<script type="text/javascript" src="/static/tiny_mce/tiny_mce.js"></script>
<script type="text/javascript" src="/static/tiny_mce/textareas.js"></script>
```

-  添加 Django admin 后台支持
在 Python26/Lib/site-packages/django/contrib/admin/templates/admin/base.html 加入上面2行代码。

#### 非Django项目在第3步之后在需要的页面引入tiny_mce.js、textareas.js即可。
