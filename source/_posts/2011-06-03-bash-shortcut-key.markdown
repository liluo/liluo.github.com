---
layout: post
title: "bash ctrl/alt 组合快捷键"
date: 2011-06-03 22:40
comments: true
categories: [Linux, Bash]
---

Ctrl/Alt 有很多实用的组合快捷键，Mark.

<!-- more -->

## Ctrl
``` bash
ctrl-a/ctrl-e
移动光标到行首/行尾,同Home/End 键
ctrl-b/ctrl-f
后移/前移 一个字符。
ctrl-c
杀死当前进程。
ctrl-d
杀死当前 Shell。
ctrl-h
删除左边一个字符, 同 Backspace 键
ctrl-l
清屏, 同 clear
ctrl-r/ctrl-R
从之前键入过的历史命令中搜索
ctrl-u/ctrl-k
删除光标前/光标后的所有字符
ctrl-xx
让光标在当前位置和行尾切换
ctrl-y
撤消前一次删除
ctrl-z
挂起当前进程,之后可以使用 fg 命令唤醒
```

## Alt
``` bash
Alt-< Alt->
在输入历史中的 最头/最尾 命令。
Alt-?
显示当前的补全列表。
Alt-*
插入所有可能的补全。
Alt-/
(无用)
Alt-。
把前面的命令行参数插入到当前位置。
Alt-b /Alt-f
后移/前移 一个单词
Alt-c
把当前字符改成首字母大写, 同时光标移到词尾
Alt-d
删除当前位置到词尾的字符
Alt-l/Alt-u
把当前单词变成全 小写/大写, 同时光标移到词尾。
Alt-n /Alt-p
在搜索历史中搜索
Alt-r
删除全部键入的内容。
Alt-t
光标前两个单词的位置互换, 并将光标移到词尾
Alt-Backspace
删除光标前一个单词, 同 Ctrl-w 一样
```
