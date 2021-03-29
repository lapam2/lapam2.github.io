---
title: git使用和如何写blog
---

---

## git使用

```bash
#1、将远程仓库克隆到本地
git clone https://gitee.com/gitee用户名/HelloGitee.git

#2、在本地修改代码，然后将修改的项目添加到git 暂缓区
#3、commit提交并备注提交信息
#4、push 将本地推送到远程仓库，完成
$ git add . #将当前目录所有文件添加到git暂存区
$ git commit -m "my first commit" #提交并备注提交信息
$ git push origin master #将本地提交推送到远程仓库
```

<!--more-->

## 如何写blog

- 利用 hexo + github 搭建

- 第一次搭建过程参考 《如何搭一篇个人博客》 
- 第二次，再添加内容时，此时相比第一次操作要简单，不需要在执行命令，记住所有命令  在  blog目录路径下执行！

```bash
#初始化 hexo，即在blog目录下建立hexo网站, hexo (init) : Create a new Hexo folder.
C:\Users\pany\blog>hexo init
```

以上不需要！

启动hexo,

```bash
# 在blog目录下启动hexo (server) : Start the server
C:\Users\pany\blog>hexo s 
```

对于第二次写博客， 这步也不需要！！

---

**从这里正式开始啦！！！**

第二次写博客的方式此处开始！！

新建博客文章标题

```bash
在blog 目录路径 下执行 新建博客标题命令
# 创建一篇新的博客标题 hexo ( new ): Create a new post.
C:\Users\pany\blog>hexo n "我的第二篇博客文"
```

- 写新的博客，进入 在C:\Users\pany\blog\source\_postsd路径下 新建的标题的 “ 我的第二篇博客文.md " 文件，编写md笔记，写完更改完成。

- 终端命令行：在哪个目录下执行以下命令？依然是 C:\Users\pany\blog>

- ```bash
  #依然是 C:\Users\pany\blog>路径下
  执行对hexo的重新生成hexo (generate) : Generate static files，和  部署hexo（deploy)
  #重新生成 hexo (generate)
  C:\Users\pany\blog>hexo g 
  #将本地仓库的个人网页部署到GitHub远程仓库上 hexo（deploy)
  C:\Users\pany\blog>hexo d
  #完成！
  ```

- 访问GitHub远程仓库，即(用户名.github.io)网址访问博客，
  例如我的网址为 lapam2.github.io

- 以上就是俺的博客地址

- lapam2.github.io   其实就是俺github上的仓库名；

- lapam2也是俺github的用户名

## 改过GitHub远程仓库地址

更改 GitHub远程仓库，则需要更改 blog目录里的  _config.yml文件 的配置，repo仓库需更改！！

不然hexo deploy 时博客内容不会更新, 需要注意！！

> ![image-20210320193655804](/pic/image-20210320193655804.png)