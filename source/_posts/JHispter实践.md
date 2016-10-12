---
title: JHispter实践
date: 2016-07-07 11:10:07
tags: JHipster
---

### JHipster 介绍

JHipster 是一个Java web的全栈项目，JHipster 的英文意思是 Java 潮人，注意它的前两个字母是大写。他以Spring boot做后台支撑，Angular2 做前端框架，集代码生成，提供多种技术方案选项，从对多种数据库的支持，到 docker 、Kubernetes 等最新技术，它值得每个 Javaer 都去研究的框架。

<!-- more -->

### 环境准备

* 服务端：
Java环境，jdk1.8(里面用到很多java8 的新特性)
依赖管理工具：maven 或gradle
数据库：mysql 或者 mongodb 、cassandra 、PostgreSQL、Oracle

* 前端：
安装Node.js ，npm 包管理器，然后npm 安装下面几个模块：

```
npm install -g yo      ## Yeoman ,更新npm install -g  yo 
npm install -g bower    ## Bower  
npm install -g gulp     ## gulp
npm install -g generator-jhipster  ##JHipster 生成器 
npm install -g jhipster-uml  ## JHipster uml 工具
npm install -g generator-jhipster to update  ## 更新JHipster 生成器
```


### 项目构建

* 生成项目
```
yo jhipster
yo jhipster:import-jdl your-jdl-file.jh
yo jhipster:server
yo jhipster:client
```

后面会有16个问题，其中第一个问题的选项：

    * monolithic 	application   #单体架构应用
    * microservice  application   #微服务应用
    * microservice  gateway       #微服务网关
    * [beat] JHipster UAA server  #(使用OAuth2 作为认证机制的微服务)

这里我们它推荐我们先选择单页应用，比较简单入手，适合创建小型 Web 站点。

* 项目配置,后台主要是spring boot 相关配置，其中 metrics 和 logstash 都是关闭的。


### 项目启动

* 服务端 根据构建工具运行启动命令
```
mvn spring-boot:run -Pprod
gradle bootRun
```
 
* 前端 gulp
gulp 是一个前端构建打包工具。
```
gulp        #默认是运行gulp server ,可以开启热加载模式，用作在开发模式下。
gulp build  #在target下生成被压缩过后的www 镜头资源文件，这可以用于生产环境。
```

### 其它
测试框架

* gatling:Gatling: 基于Scala、Akka和Netty的开源负载测试框架

* protractor:是为Angular JS应用量身打造的端到端测试框架。




