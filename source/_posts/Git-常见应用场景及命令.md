---
title: Git 常见应用场景及命令
date: 2016-10-10 18:23:22
tags: Git
---

Git 在日常开发中用的非常普遍，对于一些用的比较少的命令，总是用到再查，然后又忘记，不想这样，由此写了这篇文章。


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