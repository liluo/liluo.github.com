---
layout: post
title: "Remote 之工具篇"
date: 2013-11-13 00:02
comments: true
categories: [Remote, Tools, Douban]
---

最近从北京转到了上海 Office, 与北京技术团队的沟通协作也自然的变成了 Remote。截止到现在已经有一个月多的时间，分享一下自己使用到的工具。


<!-- more -->


### Code

![Code logo](http://i1170.photobucket.com/albums/r539/liluoliluo/logo1_zps6f5124ed.png)

Code + PullRequest 的开发流程，完全没有距离上的差异。

*Code 是豆瓣内部的代码托管工具(类似 Github)，具体介绍可以看 [<Code, 豆瓣工程师乐园>](http://www.slideshare.net/qingfeng/code-13367019)、 [<工程师文化中的工具“情结”>](http://www.qconshanghai.com/node/383)。*


### Skype

![Skype logo](http://i1170.photobucket.com/albums/r539/liluoliluo/skype-logo_zps4c5d4646.png)

[Skype](http://www.skype.com/en/) 是我们平时使用比较多的 IM(另外还有纯工程师交流 IRC)，除了方便的群组和文字聊天功能外，语音和视频也很棒。

工作中讨论、交流，每周五参加 Firelabs 分享全靠它，是目前体验到最好的语音和视频工具(Hangouts 卡顿蛮严重的……)。

另外还有一个实用的共享屏幕(Share Screen)，比如说参加 Firelabs 时看不清投影时可以请分享者共享屏幕给自己。

### Tmux

![Tmux](http://i1170.photobucket.com/albums/r539/liluoliluo/042e2390-5a4b-4cb2-9179-440989033f02_zpsa60cd8e5.jpg?t=1384268363)

之前仅仅使用 [Tmux](http://tmux.sourceforge.net/) 作为终端复用 + 分屏的工具，最近发现其在结对编程方面也是一大利器!!!


使用 Tmux 进行结对编程(需要双方使用各自账号登陆到同一台开发机)：
```
Allow another user access to your tmux session:
# specify the name of your tmux socket with -S when creating it
tmux -S /tmp/pair
# chmod to allow other users to access it
chmod 777 /tmp/pair
 
# now the other user can connect with
tmux -S /tmp/pair attach
```

### TeamViewer

![TeamViewer logo](http://i1170.photobucket.com/albums/r539/liluoliluo/teamviewer_zpsaf2997cf.png)

[TeamViewer](http://www.teamviewer.com/en/index.aspx) 是跨平台的远程访问和远程支持一体化解决方案, 甚至可以使用手机上的客户端来控制笔记本，强烈推荐用一下。


### WeChat

我是最近才开始使用微信的，除了即时、便利之外，和北京的小伙伴们偶尔在群里玩个真心话大冒险的也很有趣，哈哈~



最后，看到自己的博客已经长草了真是……

__END__
