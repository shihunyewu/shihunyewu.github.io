---
layout:     post
title:      "spring cloud start"
subtitle:   "spring cloud 入门"
date:       2019-05-07 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - Java
    - spring-cloud
    - spring-boot
---
## spring-cloud
### 基本组成
##### 1. 配置服务 Config Server
在分布式系统开发中外部配置的功能，通过 Config Server 可以集中存储所有引用的配置文件。

##### 2. 服务发现
- 目的，主要为了让每个服务之间可以互相通信
- 实现，Spring Cloud 服务发现默认实现是 Eureka， Eureka Server 作为微服务注册中心。

###### 结构图
在 [springcloud(二)：注册中心Eureka](http://www.ityouknow.com/springcloud/2017/05/10/springcloud-eureka.html) 中介绍了Eureka 的结构示意图 ![结构示意图](http://www.itmind.net/assets/images/2017/springcloud/eureka-architecture-overview.png)
主要结构有三部分：
- Eureka Server
	- 微服务注册中心，提供注册和服务发现
- Service Provider
	- 服务提供者
	- 将服务注册到 Eureka
- Service Consumer
	- 服务消费者
	- 从 Eureka 上获取服务提供者的服务

###### 容灾策略
可以设置注册中心集群。


##### 3. 路由网关
- 目的，为了让所有的微服务对外只有一个接口，我们只需要访问一个网关地址，即可由网关将我们的请求代理到不同的服务中。
- 实现，Zuul，Zuul 支持自动将路由映射在 Eureka Server 上注册服务。

##### 4. 负载均衡
- 实现，Spring Cloud 提供了 Ribbon 和 Feign 作为客户端的负载均衡。

##### 5. 断路器
- 目的，为了解决当某个方法调用失败时，调用后备方法来替代失败的方法，借此达到容错、组织级联错误等功能
- 实现，Circuit Breaker，

### 实例




##### 参考
[spring-cloud](https://github.com/ityouknow/spring-cloud-examples)
