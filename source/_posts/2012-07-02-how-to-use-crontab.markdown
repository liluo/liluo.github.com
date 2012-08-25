---
layout: post
title: "Crontab 使用入门"
date: 2012-07-02 00:22
comments: true
categories: [Linux, Shell]
---
*半夜睡不着无聊更新下博客……*

### Crontab 简介
crontab命令常见于Unix和类Unix的操作系统之中，用于设置周期性被执行的指令。该命令从标准输入设备读取指令，并将其存放于“crontab”文件中，以供之后读取和执行。该词来源于希腊语 chronos(χρόνος)，原意是时间。通常，crontab储存的指令被守护进程激活， crond常常在后台运行，每一分钟检查是否有预定的作业需要执行。这类作业一般称为cron jobs。(引用 [百度百科](http://baike.baidu.com/view/1229061.htm "crontab") )

### Crontab 使用方法
man crontab 一下会发现有以下2种用法:
``` bash
crontab [-u user] file
crontab [-u user] { -l | -r | -e }
# 其中 -u user 为可选项，当无 -u 项时为当前用户
```
第1行用法，如果 file 文件存在，则将文件复制到 crontabs 目录；如 file 不存在则接受标准输入方式。

第2行用法有3个选项：
``` bash
-l 列出当前 crontab 配置(Display the current crontab on standard output)
-r 删除当前 crontab 配置(Remove the current crontab)
-e 编辑当前 crontab 配置(Edit the current crontab using the editor)
```
<!-- more -->

### Crontab 使用语法
Crontab 语法很简单，为 "周期 命令" 的形式，具体如下：
``` bash
*  *  *  *  *  command to be executed
┬  ┬  ┬  ┬  ┬
│  │  │  │  │
│  │  │  │  │
│  │  │  │  └───── day of week (0 - 6) (0 is Sunday, or use names)
│  │  │  └────────── month (1 - 12)
│  │  └─────────────── day of month (1 - 31)
│  └──────────────────── hour (0 - 23)
└───────────────────────── min (0 - 59)
```
上面例子中 "\*" 依次代表的是分、时、日、月、礼拜几，看一下周期时间的具体格式：

``` bash
Field name  Mandatory?  Allowed values  Allowed special characters
Minutes       Yes         0-59               * / , -
Hours         Yes         0-23               * / , -
Day of month  Yes         1-31               * / , - ? L W
Month         Yes         1-12 or JAN-DEC    * / , -
Day of week   Yes         0-6 or SUN-SAT     * / , - ? L #
Year          No          1970–2099          * / , -
```

特殊符号说明：
``` bash
* cron表达式匹配的字段的所有值, 例如第4列用*表示每月
/ 描述范围的增量, 如第1列使用 */15 表示每15分钟执行一次
, 表示是一个List，如第1列使用 10,20,30 表示在每小时的第10、20、30分钟执行
- 定义范围，如第4列使用 1-5 表示在每年的1～5月执行
? 只在日期和星期字段出现，表示无意义的值，相当于定位符
L 只在日期和星期字段出现，如在日期字段表示当月最后一天，在星期字段则表示星期6（如果星期字段为4L则为最后当月最后一个星期5).
W 只在日期字段出现，如 1W 表示离1号最近的工作日
# 只在星期字段出现，如 4#3 表示当月第3个星期5
```

一些例子：
``` bash
每年1月1日0点0分 0 0 1 1 *
每月1日0点0分    0 0 1 * *
每周日0点0分     0 0 * * 0
每天0点0分       0 0 * * *
每小时0分        0 * * * *
```

### Tips
**command 标准输出发送 email**
``` bash
30 7 * * * python test.py | mail -e -s "subject" xxxx@liluo.org
# 需配置好 send_mail
```

**命令之后重定向**
``` bash
command > file 把标准输出重定向到文件
command >> file 把标准输出追加到文件

command 1 > file 把标准输出重定向到文件
command 2 > file 把标准错误重定向到文件
command 2 >> file 把标准输出追加到文件
command 2>&1 把command命令标准错误重定向到标准输出
command > file 2>&1 把标准输出和标准错误一起重定向到文件
command >> file 2>&1 把标准输出和标准错误一起追加到文件

command < file 把command命令以file文件作为标准输入
command < file >file2 把command命令以file文件作为标准输入，以file2文件作为标准输出
command <&- 关闭标准输入
```
__EOF__

