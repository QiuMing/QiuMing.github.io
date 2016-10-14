---
title: Linux Minit 18 折腾记
date: 2016-10-13 16:12:59
tags: Linux
toc: false
---

一直记得一位老司机的话，永远不要放弃 Linux。于是乎，最近又花了些时间折腾 Linux ，在自己家的台式机上面安装了 Linux Minit 18，以下记录我在 Linux 中安装各种软件命令。

<!--more-->

## 安装 Jdk 8 

默认的好像有问题

```
sudo apt-get install oracle-java8-installer
```

## 安装 Redis

这里参考了官方推荐的 [安装方式](http://redis.io/topics/quickstart)

```
wget http://download.redis.io/releases/redis-stable.tar.gz
tar -xvzf redis-stable.tar.gz -C /data/programe
cd redis-stable
make
sudo cp src/redis-server /usr/local/bin/
sudo cp src/redis-cli /usr/local/bin/
```
## 安裝 Idea 

```
破解地址
http://idea.iteblog.com/key.php

在桌面创建启动图标

[Desktop Entry]
Name=IdeaIU
Comment=Rayn-IDEA-IU
Exec=/home/rayn/idea/bin/idea.sh
Icon=/home/rayn/idea/bin/idea.png
Terminal=false
Type=Application
Categories=Developer;

```

## 安装 Sublime text 3

* 下载地址 http://www.sublimetext.com/3

* 启用方法： 控制台 输入 subl

* 安装包管理，cttl +`
```
import urllib.request,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```

* 修复中文输入问题

```
cd ~/gitgub && git clone https://github.com/lyfeyaj/sublime-text-imfix.git Maybe you should firstly run the “sudo apt-get install git”
cd sublime-text-imfix && ./sublime-imfix
```

[更多参考](http://zhidao.baidu.com/link?url=7gZcSGqT2qgbV5mdysbX7OvoBkgc7l-HySrMiEEQJBeaznA1ZBCye8Hp5Tf_6FbHjUs3A6KCw7OOtpnvWBk2SgK8zIuukMb8a9NvoV4pkQq)

## 安装 Mysql
```
sudo apt-get install mysql-server mysql-client  
sudo apt-get install libmysqld-dev 
```

##  安装 Virtualbox
```
wget http://download.virtualbox.org/virtualbox/5.1.6/virtualbox-5.1_5.1.6-110634~Ubuntu~xenial_amd64.deb

```

## 安装 Gradle
```
sudo apt-get install gradle
```

## 安装 Tomcat8
```
wget http://apache.mirrors.tds.net/tomcat/tomcat-8/v8.5.5/bin/apache-tomcat-8.5.5.tar.gz

tar -zxvf redis-3.2.3.tar.gz -C /home/ming/data/program 

```

## 安装 Nginx
```
参考
http://www.kaijia.me/2013/05/ubuntu-latest-nginx-repo-collection/ 

http://www.jianshu.com/p/ec9f2391c83d
```

## 安装 MongoDB
```
sudo apt install mongodb
```

## 安装 Chrome
```
首先下载 Chrome 浏览器 ，我是通过 火狐设置代理后去官网下载
/usr/bin/google-chrome-beta %U --proxy-server="SOCKS5://127.0.0.1:1080"
```

## 安装 Conky-manager
```
sudo apt-get install conky-manager
```

## 安装 ss-qt
```
sudo add-apt-repository ppa:hzwhuang/ss-qt5
sudo apt-get update
sudo apt-get install shadowsocks-qt5
```

## 安装 shadowsocks

[linux-ubuntu使用shadowsocks客户端配置](https://aitanlu.com/ubuntu-shadowsocks-ke-hu-duan-pei-zhi.html)


## 安装搜狗输入法
先安装 fcitx 等组件，命令如下：

```
sudo apt-get install fcitx fcitx-table-wubi-large fcitx-ui-classic fcitx-module-kimpanel
```

然后下载 下载地址 http://pinyin.sogou.com/linux/

## 安装 Wine
```
sudo add-apt-repository ppa:wine/wine-builds
sudo apt-get update 
sudo apt-get install --install-recommends winehq-devel
```

## 安装 Winetricks

```
sudo apt-get install    winetricks
```

## 使用 Winetricks 安装语言包

```
winetricks dotnet20 vjrun20
```

## 启动 QQ
```
WINEDEBUG=-all wine ~/.wine/drive_c/Program\ Files/Tencent/QQLite/Bin
/QQ.exe
```


## Leanote 笔记

* 下载地址 http://pan.baidu.com/s/1bpDl4eF

##  nvm 安装 

* 下载地址 http://www.kancloud.cn/summer/nodejs-install/71975
