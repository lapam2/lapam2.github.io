---
title: Linux常用命令
date: 2021-03-26 11:40:55
tags:
---

---

## Linux常用命令：

- 初来乍到

```bash
#自言自语
% echo hello world
% echo $PWD / echo $HOME / echo ~

#我在哪 pwd （Print Working Directory )
% pwd

#换个地方 cd ( Change Directory )
% cd path / cd ~
% cd .. #返回上一级目录
% cd ./ #当前目录
% cd /  #切换到根目录

#瞅瞅 ls ( List Directory Contents)
% ls 
% ls -l
% ls -l -a
% ls -lh
```

<!--more-->

- 文件内容

```bash
#打印文件内容 cat （Concatenate and print files )
% cat a.txt
% cat a.txt b.txt
% cat < a.txt

#头和尾 head and tail
% head a.txt
% tail a.txt

#交互浏览 less （readonly version of vi )
% less a.txt

#内容查找  / or grep
% man less | grep -n sim

#单词统计 wc ( Word, line and byte count )
% man wc | wc
```

- 重定向和管道

```bash
#重定向：改变输入输出设备
% echo hello > hello.txt
% echo world >> hello.txt
% cat < hello.txt

#管道：将前一个命令的标准输出作为下一个程序的标准输入
% man less | grep sim
% man less | grep -n sim | grep That > that.txt

```

- 寻求帮助

```bash
#用户手册 man (Manual)
% man pwd
% man man

#内置帮助 命令 -h
% man -h
% grep --help
```

- 面向StackOverflow编程



## win常用的DOS命令：

```bash
#盘符切换到d盘   
d:

#查看当前目录下的所有文件 directory
dir

#切换目录（change directory)
cd   
		
#强制切换到f盘的temp文件夹   
cd /d f:\temp
#切换到上一级  
cd..

#清理屏幕 (clear screen)  
cls  

#退出终端   
exit

#查看电脑的ip (ip configure)   
ipconfig   

#ping命令  (得到百度的ip地址，可以测试网络是否正常)   
ping www.baidu.com

#打开应用  
calc      #(计算机） 
mspaint   #（画图）
notepad   #（记事本）

#文件操作命令	
#创建文件夹(make directory)
md 文件夹名（目录名）
#删除文件夹(remove directory)	 
rd 文件夹
#创建文件 	
cd>文件名.文件后缀
#删除文件 	
del 文件	
#注：在命令提示符里鼠标右键就是粘贴
```

- DOS（Disk Operating System），磁盘操作系统；操作直接对我负责；不会理睬Windows操作系统。
- CMD（Commend，命令提示符；受操作系统保护，操作直接对系统负责；不能删除系统文件；是输入部分有需求的DOS命令的提示位置。打开CMD：Win+R  cmd 

​       查看win的版本号：win+R  -- winver

##  键盘快捷键：

键盘功能键：tab shift ctrl alt 空格 enter windows 
键盘快捷键：全选 复制 粘贴 撤销 保存 关闭窗口 运行 永久删除

tab切换菜单 ；shift组合键 ctrl控制键 alt组合键

ctrl+c	复制
ctrl+v	粘贴
ctrl+a	全选
ctrl+x	剪切
ctrl+z	撤销
ctrl+s	保存
alt+f4	关闭窗口
shift+delete	永久删除
win+r	运行
win+e	打开我的电脑       
ctrl+shift+esc	打开任务管理器
win+tab	切换应用程序

