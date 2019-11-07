---
layout:     post
title:      "maven"
subtitle:   " 项目管理工具——maven "
date:       2019-06-03 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - maven
    - 项目管理
---

## Maven 概述
Maven 是一个常见的 java 构建工程工具，首先对 maven 有个大体上的认识：
1. maven 本质上是一个独立的软件，虽然很多 IDE 都集成了很多 maven 的操作，但是 maven 和 IDE 本身无关。
2. maven 自身也是可以安装插件的，就像 vs code 编辑器可以有很多不同的插件一样。
3. pom.xml 是 maven 针对当前项目的配置文件，其中 pom 的全称为 Project Object Model，项目对象模型，可以看出其本身存在的目的就是为了构建整个项目。其中基本的，配置文件中不仅可以定义依赖的 jar 包，还可以定义 maven 所依赖的插件，当前项目的目录结构以及对项目做版本控制等。
4. maven 有一个集中管理库文件的仓库，仓库的路径可以在 maven 安装目录下的 setting.xml 中配置。

## Maven 常见标签

## Maven 常用的命令
#### 1. 创建一个项目
```java
mvn archetype:generate -DgroupId=com.mycompany.example -DartifactId=Example
```
#### 2. 将当前项目安装到本地仓库中
```java
mvn install
```

#### 参考
1. [Maven 基本操作命令](https://www.cnblogs.com/yanyd/p/4266524.html)
2. [maven常用命令介绍](https://www.cnblogs.com/adolfmc/archive/2012/07/31/2616908.html)
3. [Maven教程](https://www.yiibai.com/maven)
4. [Maven Getting Started Guide](http://maven.apache.org/guides/getting-started/index.html)