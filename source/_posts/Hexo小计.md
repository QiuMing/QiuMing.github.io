---
title: Hexo小计
date: 2016-01-27 12:56:29
toc: true
tags: 博客 Hexo
---

因为选择了技术这条路，也选择了通过 Hexo 来写博客，所以，这篇小记，用来记录笔者在使用 Hexo 的常用命令，以及遇到的一些问题。

<!--more-->

## 常用命令
```
hexo new "postName" 		#新建文章
hexo new page "pageName"	#新建页面
hexo generate		#生成静态页面至public目录
hexo server		#开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
hexo deploy		#将.deploy目录部署到GitHub
hexo help 		#查看帮助
hexo version		#查看Hexo的版本
hexo deploy -g 		#生成加部署
hexo server -g		#生成加预览
```


## 命令的简写
```
hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy
```


## 更换主题
* 下载主题放到 themes 文件夹中，在 _config.ym 中修改 theme 的名字

## 异常

* ERROR Deployer not found: git 或者 ERROR Deployer not found: github
* 解决方法： npm install hexo-deployer-git --save 
* deploy 里面的 type 值为 git
