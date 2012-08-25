---
layout: post
title: "使用 Git submodule 和 vim-pathogen 管理、同步 Vim 插件及配置"
date: 2012-05-29 20:24
comments: true
categories: 
---

在多台机器（比如 Mac 和多台服务器、开发机）上使用 Vim 经常需要同步配置文件 .vimrc，之前都是用:
``` bash
scp .vimrc theoden:~
```
如果加上形形色色的插件，再比如给 plugin 升级就有些兵荒马乱的麻烦了。今天早上在看 [vim-ruby](https://github.com/vim-ruby/vim-ruby/) 的时候发现了很 Cool 的项目 [vim-pathogen](https://github.com/tpope/vim-pathogen/)，并且使用它顺利的搞定了 Vim 的配置及插件同步。其中同步使用的是 Github + Git submodule，插件管理使用 vim-pathogen(其实 vim-pathogen 也是 Vim 的一个插件，只不过这是一个管理插件的插件)，下面记录一下整个过程。

<!-- more -->

#### 备份原来的 .vimrc 文件和 .vim 目录
``` bash
mv .vimrc{,.bak} # .vimrc -> .vimrc.bak
mv .vim{,.bak}   # .vim -> .vim.bak 
```

#### 新建 .vim/bundle 目录
``` bash
mkdir -p ~/.vim/bundle
```

#### 初始化 Git 仓库
``` bash
cd ~/.vim
git init
git add .
git commit -m "init"
```

#### 将代码托管到 Github
登陆到 <https://github.com/> ，新建一个 Repositorie 命名为 [dotvim](https://github.com/liluo/dotvim/)，然后执行：
``` bash
git remote add origin git@github.com:liluo/dotvim.git # 请改成自己的代码仓库地址
git push origin master
```

#### 安装并配置启用 vim-pathogen 插件
``` bash
git submodule add git://github.com/tpope/vim-pathogen.git bundle/vim-pathogen
echo -e "runtime bundle/vim-pathogen/autoload/pathogen.vim\ncall pathogen#infect()\n" >> ~/.vim/vimrc
# 如果想使用原来的 .vimrc 文件则可以
# cp ~/.vimrc.bak ~/.vim/vimrc
# 但是必须将以下2行加到 vimrc 的首行
# runtime bundle/vim-pathogen/autoload/pathogen.vim
# call pathogen#infect()
ln -s ~/.vim/vimrc ~/.vimrc
```

#### 安装插件
``` bash
#git submodule add 插件 Git 仓库地址 bundle/插件名字
git submodule add git://github.com/tpope/vim-markdown.git bundle/vim-markdown
```

#### 升级插件
``` bash
cd ~/.vim/bundle/vim-markdown # 将 vim-markdown 替换为需要升级的插件名字
git pull origin master
```

#### 升级所有插件
``` bash
cd ~/.vim
git submodule foreach git pull origin master
```

#### 删除插件
``` bash
git rm bundle/vim-markdown # 将 vim-markdown 替换为需要升级的插件名字
```

#### 将整个目录提交到 Github
``` bash
cd ~/.vim
git push origin master
```

#### 在其他机器使用相同配置
``` bash
git clone http://github.com/username/dotvim.git ~/.vim
ln -s ~/.vim/vimrc ~/.vimrc
cd ~/.vim
git submodule init
git submodule update
```
