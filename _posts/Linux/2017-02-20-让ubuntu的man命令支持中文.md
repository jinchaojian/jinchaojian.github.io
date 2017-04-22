---
layout: post
title: 让ubuntu的man命令支持中文
category: Linux
tags: Linux
description: 让ubuntu的man命令支持中文
---
在使用linux时，会碰到很多不是很熟悉的命令，这时通常需要使用man来查阅一些命令的帮助信息。

但是默认的语言是英文的，这里记录一下让ubuntu的man命令支持中文的方法。

ubuntu源里面已经包含了中文的man包，所以不用从其他地方down了，直接    

sudo apt-get install manpages-zh    

即可。

这时运行man ls 即可看到中文的说明。
