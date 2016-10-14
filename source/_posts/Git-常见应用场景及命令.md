---
title: Git 常见应用场景及命令
date: 2016-10-10 18:23:22
tags: Git
---

Git 在日常开发中用的非常普遍，对于一些用的比较少的命令，总是用到再查，然后又忘记，反复如此其实挺浪费时间的，因此写了这篇文章，做些笔记。


<!--more-->

## 在 github 上 fork 了别人的项目后，及时获取最新代码
我是这样做的，在 .git 文件夹中的 config  文件中，添加了下面的代码
```
[alias]
    update = pull git@github.com:people_name/project_name.git master
```
以后每次更新的时候就运行
```
git update 
```
嗯，没错，就是利用了 alias  来偷懒输入那么长的地址。

## 重新设置远程仓库链接地址或者协议

* 比如我们在 git 上新建了或者 fork 了别人的项目，默认是走 http 协议，这样需要每次 push 都需要输入密码，而我实际上已经在 gihub 上传了我的公钥，我可以走 ssh 协议，不需要输入密码。

* 解决命令
```
git remote set-url origin  git@github.com:QiuMing/project_name.git
```

## 删除已经 push 到远程仓库但需要忽略的文件

* 比如不小心吧 .idea 这个的文件不小心 push 到 git 上，需要删除。

* 解决命令
```
$ echo '.idea' >> .gitignore  ##添加到 .gitignore

$ git rm —cached -r .idea   ##从 cache 中删除

然后 add  commit  psuh
```

