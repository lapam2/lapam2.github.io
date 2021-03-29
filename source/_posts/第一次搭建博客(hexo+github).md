---
title: 第一次搭建博客（hexo+github）
date: 2019-08-24 07:50:35
tags:
---

---

## win下环境配置

Node.js下载（见官网）

Git下载

<!-- more -->


---

## 建立本地仓库hexo

- 在Windows的命令行输入以下命令，安装hexo

- ```bash
  npm install -g cnpm --registry=https://registry.npm.taobao.org
  cnpm install -g hexo.cli
  ```

- 新建blog文件夹（目录），作为本地仓库使用 

- ```bash
  mkdir blog
  ```

- 命令行在 blog目录路径下 执行: 建立hexo网站

- ```bash
  hexo init
  ```

- (可选查看)启动hexo, 可以通过localhost:4000访问。不用可关掉

- ```bash
  hexo server
  ```

- 命令行在 blog目录路径下，新建博客文章标题

- ```bash
  hexo new "我的第一篇博客文"
  ```

- 编写博客内容： 在/blog/source/_posts/路径下打开新建博客文章，使用markdown编写博客内容  

- 至此，本地博客写好了，可通过本地端口（localhost:4000）访问

## 本地部署到远程仓库

- 安装hexo-deployer-git插件，为了将hexo部署到远程仓库

- ```bash
  cnpm install hexo-deployer-git --save
  ```

- 修改blog目录路径下的 _config.yml 里的配置  

- ```bash
  deploy:
      type: git 
      repo: GitHub远程仓库地址 (例如我的就是: https://github.com/lapam2/lapam2.github.io.git )
      branch： master
  ```

- 重新生成hexo 静态网页

- ```bash
  hexo generate
  ```

- 将生成的hexo 静态网页 部署到GitHub远程仓库

- ```bash
  hexo deploy
  ```

- 完成！可以通过访问GitHub上的**GitHub用户名.github.io**网址访问博客
- 例如我的就是： lapam2.github.io