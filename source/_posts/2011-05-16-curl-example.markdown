---
layout: post
title: "curl 简单作弊条"
date: 2011-05-26 22:26
comments: true
categories: [Linux, Bash] 
---
curl 是一款命令行多协议支持的服务器访问工具,可以用它来访问HTTP服务器，就像浏览器一样（当然了，它也是可以通过FTP下载或上传文件）。

<!-- more -->

### 基本应用
``` bash
curl http://www.douban.com
```
上面的命令会在屏幕上输出服务器的响应信息，需要headers信息加 -i/--include 参数，只需要headers信息可以使用 -I/--head。


重定向输出：
``` bash
curl http://www.douban.com > response.html
curl http://www.douban.com | grep 'douban.com'
```

如果使用管道，默认会有一个进程信息显示出来，可以使用 -s/--silent 来不显示它们：
``` bash
curl -s http://www.douban.com | grep 'douban.com'
```

如果想保存服务器返回的内容的话，除了使用 > 重定向到一个文件外，还可以使用 -o/--output 参数指定需要保存到的文件：
``` bash
curl http://www.douban.com -o response.html
```

非文本文件也能这样保存：
``` bash
curl http://img3.douban.com/pics/nav/lg_main_a7.png -o logo.png
```

原名保存使用 -O/--remote-name 选项：
``` bash
curl http://img3.douban.com/pics/nav/lg_main_a7.png -O
```
不过豆瓣的图片有简单的防盗链，所以可能下载不成功 : ( 继续往下看

### 发送数据
GET 方法的请求没什么特殊的，直接在 url 中放上数据就可以了：
``` bash
curl http://www.douban.com/?name=luo
curl http://www.douban.com/?name=小落
```

POST 方法的话就需要使用 -d/--data 参数，只要有这个参数，即使值是空字符串，那么出去的就是 POST 方法的访问：
``` bash
curl -d 'name=luoluo&passwd=*****' http://www.douban.com
```

将文件以二进制流数据 POST
``` bash
curl -F image=@/Users/luo/img1.jpg http://localhost:4567
```

### 头部信息
先说最常构造的两个 User-Agent 和 Referer ，这两个分别使用 -A/--user-agent 和 -e/--referer 来指定：
``` bash
curl -A Chrome http://www.douban.com
curl -e http://liluo.org http://www.douban.com
```

包含这两个头部信息，所有的头部信息参数都可以使用 -H/--header 来设置：
``` bash
curl -H Referer:http://liluo.org http://www.douban.com
curl -H User-Agent:Chrome -H Accept-Language:zh-cn http://www.douban.com
```

### COOKIE控制
curl 是可以支持带 cookie 的交互行为的。使用方式是 -D/--dump-header 用于指定一个文件保存获取到的 cookie 信息（实际上包含了整个头部信息）， 然后用 -b/--cookie 指定一文件用于读取保存的 cookie 。
``` bash
curl http://www.douban.com -D cookie.txt
curl http://www.douban.com -b cookie.txx
```
-D 保存出来的头部信息就是以纯文本形式存放的，所以，你可以方便地随便修改。

### 代理和通配符

#### 代理设置

使用 -x/--proxy 参数：
``` bash
curl -x http://web.proxy.url http://www.douban.com
```

#### 通配符
``` bash
curl -O http://www.douban.com/~liluo/screen[1-10].jpg
curl -O http://www.douban.com/~{liluo,luoluo}/[001-201].jpg
```

#### 反向引用分组
``` bash
curl -o #2_#1.jpg http://www.douban.com/~{liluo,luo}/[001-201].jpg
```
