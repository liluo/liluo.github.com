---
layout: post
title: "使用Python/Ruby/Bash Post 文件二进制流（stream）"
date: 2012-05-04 23:14
comments: true
categories: [Python, Ruby, Bash, Linux] 
---
之前经常使用Python的urllib, urllib2两个库写爬虫或者是向第三方接口post数据，很是清爽。今天有需求要把图片文件post到第三方接口，当我又祭出urllib+urllib2两大法宝时，结果很不意外的被洗刷刷了……

被洗刷的感觉很不爽，需求是流氓，你弱它就强，所以一定要搞定它 XD

各种搜罗和实践测试，最后得到了python\ruby\bash几个版本：

<!-- more -->

## Pyhton版1

使用第三方库poster, 安装：
``` bash
easy_install poster
```

代码:
``` python
from poster.encode import multipart_encode
from poster.streaminghttp import register_openers
import urllib2

register_openers()

datagen, headers = multipart_encode({
                     'image': open('/Users/luo/img1.jpg', 'rb')
                   })

request = urllib2.Request('http://localhost:4567/', datagen, headers)
print urllib2.urlopen(request).read()
```
参考：<http://atlee.ca/software/poster/index.html>

## Python版2

使用第三方库requests（同事 Tachikoma 推荐），安装：
``` bash
easy_install requests
或者
pip install requests
```
代码：
``` python
import requests

url = 'http://localhost:4567'
files={'image': ('img1.jpg',open('/Users/luo/img1.jpg', 'rb'))}

r = requests.post(url, files=files)
```
参考：<http://docs.python-requests.org/en/latest/user/quickstart/>

## Ruby版

使用gem包rest-client，安装：
``` bash
gem install rest-client
```
代码：
``` ruby
require 'rest_client'
RestClient.post('http://localhost:4567/', 
                :image => File.new('/Users/luo/img1.jpg'))
```
参考：<https://github.com/adamwiggins/rest-client>

## Bash版

使用curl，安装：
``` bash
不用讲了吧？
```
代码：
``` bash
curl -F image=@/Users/luo/img1.jpg http://localhost:4567
```
参考：
``` bash
man curl
```

### 测试的服务器端代码:
``` ruby
require 'sinatra'
post '/' do
  data = params['image'][:tempfile].read
  f = File.new('image1.jpg', 'w')
  f.puts data
  f.close
  'ok'
end
```
