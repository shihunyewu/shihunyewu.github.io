---
layout:     post
title:      "spring boot start"
subtitle:   "开箱即用的 spring boot"
date:       2019-05-07 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - Java
    - Spring
    - spring-boot
---

> spring-boot 让你用最少的配置最快地搭建出想要的应用

## Spring-boot
##### spring-boot 的优点
- 配置少，spring 框架整合其他框架的时候需要配置大量配置文件，这相比于业务开发耗费了更多的时间和精力。spring-boot 直接从项目创建时就已经整合好了对应的框架，只要配置一下必需的配置即可搭建完成一个应用环境。
- 直接运行，spring-boot 可以内置 tomcat 等服务器，直接一键运行，不必特意部署到服务器上。

### 常见的 spring-boot 组件
#### spring-boot-mvc
#### spring-boot-WebSocket
#### spring-boot-jpa
众所周知，Mybatis 相比于 Hibernate 来说更加灵活，能够自定义关系，那么较 Mybatis 更新的 spring-boot jpa 有啥优势呢？[Spring Data JPA 与 MyBatis简单对比](https://www.jianshu.com/p/3927c2b6acc0)里面简单的比较了一下。

#### spring-boot-security
认证和授权，授权即确定用户在当前系统下所拥有的功能权限。
###### Spring Security 基本配置
- DelegatingFilterProxy，用来过滤权限

#### spring-boot-JMS

##### 参考
1. [springMVC 和 spring-boot 的区别](https://www.cnblogs.com/ThinkVenus/p/8026633.html)
2. [spring jpa 和 Mybatis 的简单对比](https://www.jianshu.com/p/3927c2b6acc0)