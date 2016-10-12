---
title: 一行命令启动Web
date: 2016-05-21 10:29:41
tags: 小技巧
---

经常会遇到要在某一个目录下启动一个 Web的情景，有很多种实现方式。

其中我推荐 使用 Node.js 这种方案：

<!-- more -->
使用 Node.js 安装http-server。
```
hs -o -p 8000
```

以下，是我有用到过的命令
```
## python
python2 -m SimpleHTTPServer 8000

## python3.x
python -m http.server 8000

## node.js
npm install -g http-server
http-server -p 9000
hs -p 9000   ## 上面的还可以简写 为
hs -help    ## 更多用法

## php(>5.4)
php -S 127.0.0.1:8000
```