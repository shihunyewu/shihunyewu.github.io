---
layout:     post
title:      "Message Queue"
subtitle:   "消息队列"
date:       2019-04-30 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - Message-Queue
---

## 消息队列
### 使用消息队列的场景
[消息队列的应用场景](https://www.cnblogs.com/stopfalling/p/5375492.html)
[消息队列概述](https://www.cnblogs.com/ruiati/p/6649868.html)
#### 1. 异步处理
将串行的处理过程变成可以异步处理。比如用户注册完信息后，可以将注册完的消息写入消息队列中，向邮箱发送邮件和相手机发短信两个操作可以异步执行，这样比串行执行快。
#### 2. 应用解耦
将每个独立的过程分开，比如下单和检查库存系统两个过程分开，用户下单成功，但是订单是否完成还要看库存系统。

#### 3. 流量削峰
突然大量流量涌入，但是系统处理能力有限，又不能抛弃用户请求，可以将用户请求放进消息队列，慢慢处理。

#### 4. 日志处理
和流量削峰差不多，一个时间段用户产生大量的用户日志，将处理日志的请求放进消息队列中，慢慢处理。

#### 5. 消息通讯
消息队列最基本的功能就是消息通信


## Java 消息传递服务(JMS)
###  消息模型
#### p2p 模型
端到端的形式
#### 发布订阅(pub/sub)

### 消息消费
1. 同步接收消息
2. 异步接收消息

### 编程模型
1. 连接工厂 connectionFactory
2. 连接 connection
3. 会话 session
4. 消息生产者的消息发送目标或者说消息消费者的消息来源 destination
5. 消费者
6. 生产者
7. 消息监听器，观察者模式

### 常用的消息队列
1. ActiveMQ
2. RabbitMQ
3. ZeroMQ
4. Kafka

### 实践
#### ActivieMQ Demo
[教程](https://www.cnblogs.com/jaycekon/p/6225058.html)
- 启动 ActiveMQ 的服务，在 bin/win64 中
- 打开监控网页
- 编写生产者和消费者，分别启动

#### 抢购系统
[流量削峰](https://eric-weitm.iteye.com/blog/2429894)
[RabbitMQ+Redis集群+Quartz实现简单高并发秒杀](https://blog.csdn.net/G626316/article/details/78650508)

#### 快速答题系统

#### 大数据场景
[项目中为什么通常flume和kafka要共同使用?](https://blog.csdn.net/weixin_41919236/article/details/84522423)






