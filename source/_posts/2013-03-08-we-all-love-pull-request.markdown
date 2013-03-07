---
layout: post
title: "我们都爱 Pull Request"
date: 2013-03-08 00:14
comments: true
categories: [Git, Douban]
---

![Pull Request](http://i1170.photobucket.com/albums/r539/liluoliluo/pull-request-logo_zps575a2d31.png "Pull Request")

最近半年多，我们组一直在使用 [Pull Request](https://help.github.com/articles/using-pull-requests) 的方式进行开发，写一点儿自己的感受。

说到 Pull Request 就不得不提到 Code Review。当我们还是以 [SVN](http://subversion.tigris.org/) 为主要的版本控制时，Code Review 通常是2个人参与(提交者和另外一位同事)，为了 Review 而 Review，实在是无趣。后来也有短暂的一段 [Hg](http://mercurial.selenic.com/) 经历（这段基本忘干净了，想了几分钟都没什么印象……），直到转向战无不胜的 [Git](http://git-scm.com/)...

<!-- more -->

事情是从清风老师签名“等你的 pull request”开始的，毛线把 Pull Request 的 Web 界面、功能(评论, 行间评论)做到超赞的时候，Pull Request 的开发方式就成了我们组的主流。当代码需要 Review 的时候，"@" 上相关同事，于是大家都跳出来对 diff 中某个文件、某段代码甚至某行“评头论足”、“指手画脚”，好不热闹。

![Pull Request](http://i1170.photobucket.com/albums/r539/liluoliluo/PullRequest_zpsda28fdd7.png "Pull Request")

最初的时候，对于这种情况挺不适应的，特别是遇到自己的代码被吐槽时那叫一个脸红心跳……过了一小段时间，渐渐的没有了这种感觉（贱贱的脸皮变厚了，哈哈哈哈），就体会到 Pull Request 的好处了：


### 代码质量更有保证

参与进来的的同事更多，看问题更全面，对代码本身甚至设计理念都会有一个保证

### 更多人受益

除了代码提交者本身，参与进来的同事甚至“路过”的同事都可以从 comments 中看到整个讨论的过程

### 学习效果好

对于提交者来讲有点心理压力，印象更深刻

### 正能量

当提交很棒的功能或者漂亮的代码时会被各种赞美，于是脑子一热又多做一个需求

### 很有趣，欢乐!!!

Comments 中的各种表情、卖萌、甚至还有调戏、基情，让人更愿意参与，更多滋味更多欢笑更多奔放!!!


![Pull Request](http://i1170.photobucket.com/albums/r539/liluoliluo/694ba542-0d15-45bc-8efb-f6cf4f9449fe_zpse8fec570.jpg "Pull Requesgt")
