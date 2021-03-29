---
title: hexo错误解决
date: 2021-03-20 20:53:58
tags:
typora-root-url: ..
---

---

## ping github.com超时问题： ?

![image-20210325210226319](/pic/image-20210325210226319.png)

怎么解决？？？

<!-- more -->

原因：github服务器在国外，有时连接会缓慢。可以有以下解决办法:

**（待补充）**

---

## 从hexo hexo generate 出错：?

![image-20210325213047389](/pic/image-20210325213047389.png)

解决方案：上述错误是由于博文的yaml头部格式不正确导致的，正确格式为

```xml
title: yourtitle
description: your description
tags: 
 - tag1
 - tag2
categories: categorie1

---
以下为正文

#title的内容中间不能有空格！
#头部和正文之间要使用3个“-”进行分割，3个“-”与头部之间要有一个空行
#注意冒号: 后边要有空格，“-”符号前后都要有空格！
```

---

## hexo depoly 出错warning：

- ![image-20210320214056941](/pic/image-20210320214056941.png)

怎么解决？？

以上出现的是warning警告，可以先不用管。。。

---

## hexo部署到远端服务器github后图裂？

解决方式：

出现这个问题是找不到图片地址？（相对地址，绝对地址）使用谁？

**建议使用绝对地址**：

![image-20210325210829851](/pic/image-20210325210829851.png)

![image-20210325211009584](/pic/image-20210325211009584.png)



可以在C:\Users\pany\blog\source\pic，即将图片存储在  source\pic 目录下，因为微博的根目录是从source打开。source相当于其根目录/	此步骤可以在Typora中格式--图片--设置图片根目录下，更改图片根目录为C:\Users\pany\blog\source路径。即可。以后图片复制后路径就为  /pic/img.png 

然后更改Typora插入图片时的指定路径，即  source\pic  目录。完成！部署到github后可以访问到图片了。

![image-20210325212628057](/pic/image-20210325212628057.png)

![image-20210325211352136](/pic/image-20210325211352136.png)

![image-20210325213256868](/pic/image-20210325213256868.png)



## OpenSSL问题？每次hexo d都要输入github账户密码？

![image-20210326182439097](/pic/image-20210326182439097.png)



```bash
fatal: unable to access 'https://github.com/lapam2/lapam2.github.io.git/': Could not resolve host: github.com
FATAL Something's wrong. Maybe you can find the solution here: https://hexo.io/docs/troubleshooting.html
Error: Spawn failed
    at ChildProcess.<anonymous> (C:\Users\pany\blog\node_modules\_hexo-util@0.6.3@hexo-util\lib\spawn.js:52:19)
    at ChildProcess.emit (events.js:198:13)
    at ChildProcess.cp.emit (C:\Users\pany\blog\node_modules\_cross-spawn@4.0.2@cross-spawn\lib\enoent.js:40:29)
    at Process.ChildProcess._handle.onexit (internal/child_process.js:248:12)
```

解决方法：

后者要联网才能访问github,hexo d 部署到github 网页上。

前者解决是SSL的原因？









## 知识补充

npm&Node.js   基于JavaScript写的

maven&tomcat  基于Java写的  Servlet?  jsp?

前者都为包的管理工具，后者为运行服务器

java网络编程：http，https, tcp/ip 

java并发（即多线程开发：（线程，线程池，锁，JUC

前端 hcjs, ajax交互

nginx 反向代理

集合框架底层实现，和排序算法。。。剑指offer，源码阅读



