---
layout:     post
title:      "Java-spring-springMVC-Mybatis"
subtitle:   " \"Java ssm 框架\""
date:       2019-05-05 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - Java
---
> 好记性不如多总结，二刷基础框架

# SSM+Redis
## 第一章 认识 ssm 和 redis
### 1.1 spring 框架
#### 1.1.1 IOC
0. IOC，控制翻转，Inversion of Control
1. IOC 容器管理 java bean，还提供对 bean 的生存周期的一些管理
2. 用户代码之间的解耦，接口注册，但是接口的实现类可以改变，在 xml 配置文件中可以重新配置。
3. 被动行为，不是使用 bean 的用户直接创建 bean，而是容器观察到用户需要使用 bean 的时候，注入

#### 1.1.2 AOP
0. AOP，面向切面编程，Aspect Oriented Programming
1. 用户代码和系统代码之间的解耦，简化代码，不必为每一个用户代码都实现一份系统代码。
2. 举例，数据操作之前需要建立连接，完成操作之后，需要销毁连接。


### 1.2 Mybatis 简介
1. 优势为封装少、映射多样化、支持存储过程和可以进行 SQL 优化，这些也是相对于 Hibernate 来说的优势。
2. ORM 映射

### 1.3 Spring MVC
0. MVC 架构，控制器、视图解析器、视图和消息转换系统等。

### 1.4 Redis
0. 缓存工具，响应速度快，可以用于抢红包，商品秒杀等场景
1. 支持 6 种数据类型
2. 操作都是原子的，单线程的，可以用来做分布式锁
3. 消息传递队列，支持发布+订阅的模式

### SSM+Redis
各自承担的功能
- Spring IOC 承担了一个资源管理、整合、即插即拔的功能。
- Spring AOp 可以提供切面管理，特别是数据库事务的管理
- Spring MVC 用于把模型、视图和控制器分层，组合成一个有机灵活的系统
- Mybatis 提供了一个对于数据库访问的持久层，通过 Mybatis-Spring，能和Spring 无缝对接。
- Redis 作为缓存工具，高速处理数据和缓存数据的功能。在某些需要高速运算的场合中，可以先用它来完成运算，再把数据批量存入数据库中。

## 第二章 Java 设计模式
### 2.1 反射技术
- 本质，通过类的全限定名构造一个该类的对象
	- 也可以只反射方法
- 优点，只要配置就可以生成对象，可以解除程序的耦合程度。
- 缺点，运行慢
- 应用，Spring IOC 容器

### 2.2 动态和责任链模式
- 本质，用代理对象来访问真实对象的方法，可以对原来对象的行为进行增强，和装饰器模式类似
	- 二者的区别，使用代理模式，代理和真实对象之间的的关系通常在编译时就已经确定了，而装饰者能够在运行时递归地被构造。[代理模式和装饰者模式的区别](https://www.cnblogs.com/xiaolovewei/p/7751332.html)
- 实现方式
	- JDK，只能使用接口
		- 接口 + 实现了接口的类 C >> 使用接口引用的代理类(增强了 C)
	- CGLIB，可以使用类
- 举例，拦截器，类似于 AOP
- 责任链模式
	- 定义，多个拦截器嵌套，比如请假需要经过项目经理、部门经理、人事等多个角色的审批，每个角色都是拦截器。
	- 应用场景，用于一个对象在多个角色中传递的场景。

### 2.3 观察者模式
- 当被观察者发生改变时通知观察者
	- 举例，AIO 实现，Swing 监听器。

### 2.4 工厂模式和抽象工厂模式
#### 普通工厂模式
举例，根据产品 id 返回不同的产品。
#### 抽象工厂
举例，多个工厂，每个工厂负责生产一个系列的产品，用户在外部给总工厂产品 id，抽象工厂会根据 id 选择对应的工厂来生产对应的产品。
#### 建造者模式
- 出现原因，对象参数繁多非常复杂时，需要分步来构造对象
- 举例，KFC 的套餐，不同的套餐所包含的产品的组合不同，汉堡套餐，可以先构造可乐，再构造薯条，然后是汉堡，这时候返回的套餐对象就是汉堡套餐。如果是别的套餐，例如鸡肉卷套餐，先构造可乐、薯条，再构造鸡肉卷，这时候返回的对象就是鸡肉卷套餐。

## 第 3 章 MyBatis
### 3.3 核心组件
- SqlSessionFactoryBuilder（构造器）：根据配置产生对应的 SQLSessionFactory，采用的是分步构建的 Builder 模式，也是使用了抽象工厂来生成不同的具体工厂。
- SqlSessionFactory，产生 SqlSession，使用的是工厂模式
- SqlSession，发送 SQL 执行返回结果，获取 Mapper 的接口。
- SQL Mapper 映射器，由 Java 接口和 XML 文件构成，需要给出对应的 SQL 和映射规则。负责发送 SQL 去执行，并返回结果。

### 3.4 SqlSessionFactory
- 生成 SqlSessionFactory，可以根据 XML 或者是在 java 代码中来生成 SqlSessionFactory。
	- 推荐用 XML，改换配置，这样代码不必重新编译
#### 3.4.1 使用 XML 构建 SqlSessionFactory
- XML
	- 基础配置文件
		- 配置数据库连接
		- 联结 mapper.xml 和 接口
	- mapper.xml
		- SQL 语句，输入参数、返回参数等信息

### 3.5 SqlSession
两个实现类，DefaultSqlSession 和 SqlSessionManager
- DefaultSqlSession，单线程使用
- SqlSessionManager，多线程使用

- 作用
	- 获取 Mapper 接口
	- 发送 SQL 给数据库
	- 控制数据库事务
### 3.6 映射器
配置内容
- 描述映射规则
- 提供 SQL 语句，配置 SQL 参数类型、返回类型、缓存刷新等信息。
- 配置缓存
- 提供动态 SQL

实现方式
- 注解
- XML，推荐用 XML

### 3.7 生命周期
**生存周期是组件的重要问题**，尤其是在多线程环境中。
- SqlSessionFactoryBuilder，创建完 SqlSessionFactory 之后就失去了作用。应该销毁。
- SqlSessionFactory，被看做数据库连接池，作用是创建 SqlSession 对象。需要长期保存。一般情况下我们不需要创建多个数据库连接池，因为我们往往希望 SqlSessionFactory 作为一个单例，让其在应用中共享。
- SqlSession，数据库连接，使用完了就归还给 SqlSessionFactory
- Mapper，只有在发送 Sql 给数据库时才有用，当用完之后就应该废弃。


## 第四章 Mybatis 配置
- properties，配置供其他地方使用的属性，在其他地方使用时用 `${username}` 即可，可以配置数据库连接，用户名之类的参数
	- 也可以单独配置一个文件比如 jdbc.properties，放到 classpath 的路径下，然后在 Mybatis 中引用。
- settings
	- 自动映射方法
	- 驼峰命名映射
	- 级联规则
	- 是否启动缓存
	- 执行器类型
	- ...
- typeAliases
	- 为类配置别名
- typeHandler
	- 类型转换器，大部分情况下不需要用户去自定义数据类型转换，系统已经提前写好
		- 即使是枚举，Mybatis 也已经提前定义好了两个转换器
	- jdbcType，数据库类型
	- javaType，java 类型
	- handler，自定义的类型转换器
	- 启用
		- 隐式启用，只要 mapper 结果集中的类型转换方式类似，那么将会使用该 handler
		- 显式启用，可以在结果集中显式启用自定义的 handler
- ObjectFactory，对象工厂，不常用
	- Mybatis 会使用一个对象工厂来完成创建这个结果集实例。用户可以自定义，在生成结果集实例的过程中做一些改变。
- environments，配置环境
	- 环境变量
		- 事务管理器
		- 数据源
			- 数据库驱动
			- 数据库连接 url
			- 数据用户名和密码
			- 数据类型
				- UNPOOLED
				- POOLED
					- poolMaximumActiveConnections 在任意时间内都存在的活动连接数量，默认值为 10
					- poolMaximurnldleConnections 任意时间可能存在的空闲连接数
					- [常见配置](https://www.cnblogs.com/haw2106/p/7028069.html)
				- JNDI，？？ JNDI是Java Naming and Directory Interface（JAVA命名和目录接口）的英文简写，它是为JAVA应用程序提供命名和目录访问服务的API（Application Programing Interface，应用程序编程接口）。
- databaseProvider，数据库厂商标识，不常用
- mapper，映射器
	- mapper.xml 中引入对应的接口
	- Mybatis.xml 中引入对应的 mapper.xml
		- 用 mapper.xml 文件路径引入
		- 用包名引入
		- 用类注册引入
		- 用文件 url 引入

## 第五章 映射器
### select 查询语句

## 第六章 动态 SQL

## 第七章 Mybatis 的解析和运行原理

## 第八章 插件

## 第九章 Spring IOC

## 第十章 装配 Spring Bean

## 第十一章 面向切面编程
- 本质，AOP 面向切面编程的实现基础是动态代理
- 解决的问题，例如数据库开发中的获取数据库连接，释放数据库连接
- 基本织入方式有五种
	- 前置通知
	- 后置通知
	- 环绕通知
	- 返回通知
	- 异常通知
- spring 对 AOP 的实现方式有两种
	- JDK 动态代理
	- CGLIB 代理
- spring 配置 AOP 的方式
	- XML
	- 注解
- 多个切面
	- 执行顺序
		- @Ordered 强制有序

## 第十二章 Spring 和数据库编程





##### 参考书
`<<Java EE 互联网轻量级框架开发 SSM框架>>`
