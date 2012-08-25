---
layout: post
title: "Python 爬虫基础"
date: 2010-06-22 16:57
comments: true
categories: [Python]
---
-  基本的抓站
``` python
import urllib
content = urllib.urlopen('http://www.xxxx.com').read()
```

-  使用代理服务器
``` python
import urllib2
proxy  = urllib2.ProxyHandler({'http':'http://host:port'})
opener = urllib2.build_opener(proxy, urllib2.HTTPHandler)
urllib2.install_opener(opener)
content = urllib2.urlopen('http://www.xxxx.com').read()
```

<!-- more -->

-  Cookie 处理
``` python
import urllib2, cookielib
cookie = urllib2.HTTPCookieProcessor(cookielib.CookieJar())
opener = urllib2.build_opener(cookie, urllib2.HTTPHandler)
urllib2.install_opener(opener)
content = urllib2.urlopen('http://www.xxx.com').read()
```

-  同时使用Cookie和Proxy
``` python
import urllib2, cookielib
proxy  = urllib2.ProxyHandler({'http':'http://host:port'})
cookie = urllib2.HTTPCookieProcessor(cookielib.CookieJar())
opener = urllib2.build_opener(proxy, cookie, urllib2.HTTPHandler)
urllib2.install_opener(opener)
content = urllib2.urlopen('http://www.xxx.com').read()
```


-  POST 数据

比如说需要向 http://www.xxx.com/post/ 接口 POST 数据 name='liluo', age='21', blog='http://liluo.org'

首先需要准备数据
``` python
import urllib
data = urllib.urlencode({
    'name': 'liluo',
    'age' : '21',
    'blog': 'http://liluo.org'
})
```

然后生成并发送 HTTP 请求
``` python
req = urllib2.Request(url='http://www.xxx.com/post/', data=data)
ret = urllib2.urlopen(req).read()
``` 

-  伪装成浏览器

很多网站不喜欢爬虫（比如糗事百科），发送的请求会被拒绝。这个时候我们可以用修改 HTTP headers 信息来伪装成浏览器:
``` python
import urllib2
headers = {'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_6_8) AppleWebKit/535.19 (KHTML, like Gecko) Chrome/18.0.1025.168 Safari/535.19'}
req = urllib2.Request(
    url = 'http://www.xxx.com',
    headers = headers 
)
ret = urllib2.urlopen(req).read()
```

-  绕过“反盗链”

某些网站（再比如糗事百科）图片会有所谓的反盗链设置，其实就是检查 HTTP 请求的 headers 里的 referer 是否来自该网站。所以只需改下 headers:
``` python
headers = {'Referer': 'http://www.qiushibai.com' }
req = urllib2.Request(
    url = 'http://qiushibaike.com/',
    headers = headers
)
``` 
