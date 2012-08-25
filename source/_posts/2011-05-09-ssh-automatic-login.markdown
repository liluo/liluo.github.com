---
layout: post
title: "SSH automatic login(免密码)"
date: 2011-05-09 22:13
comments: true
categories: [Linux, Bash]
---

其实我觉得每次使用SSH时输入用户名和密码也没什么不好，但是却被同事各种无情的鄙视。 T T

### 创建公钥
``` bash
ssh-keygen -t rsa
```
无视它出来的任何提示，欢快的一路回车到底吧~

<!-- more -->

### 把公钥复制到远程主机
把公钥id_rsa.pub复制到远程机器的 /home/username/.ssh目录并命名为authorized_keys
``` bash
# 方法1, os x 可以通过 `brew install ssh-copy-id` 安装 ssh-copy-id
ssh-copy-id user@host;

# 方法2
cat ~/.ssh/id_rsa.pub | ssh user@host "mkdir ~/.ssh; cat >> ~/.ssh/authorized_keys"
``` 
多台远程主机就多次复制～ 如果你本机登陆用户和远程登陆用户一致的话，就可以直接 ssh hostname 直接登陆，下面就不用看了。

### 解决本地登陆用户与远程登陆用户不一致
好吧，这事很纠结，虽然不用输入密码了，但是还得 ssh username@hostname 来登陆，很不爽，你懂的。 其实解决也很简单（but是同事告诉我的，老脸一红），修改本地登陆用户的 ~/.ssh/config 文件，如果木有的话就自个儿建一个吧，内容如下：
``` bash
Host theoden
    user liluo
Host fili
    user liluo
Host hostname
    user name
```
这样，本地和远程登陆用户名不一致也可以 ssh hostname 登陆了。
