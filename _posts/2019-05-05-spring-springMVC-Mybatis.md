---
layout:     post
title:      "Java spring springMVC mybatis redis"
subtitle:   " \"Java ssm redis\""
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

## 第 三 章 MyBatis
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
### 9.2 IOC
- 主动创建对象
- 被动创建对象
- 阐述
	- 不需要关心所需要的对象的创建，只要能够根据描述来获得 bean 即可
	- IOC 容器来管理

### 9.3 IOC 容器
- 两个接口
	- BeanFactory
		- Spring IOC 容器所定义的最底层的接口
		- 主要的成员函数
			- getBean(...) 获取 Bean
			- isSingleton(String name) 判断是否是单例
			- isPrototype(String name) 判断是否是原型模式
	- ApplicationContext
		- 是 BeanFactory 的子接口之一，扩展了很多接口

#### 9.3.2 Spring IOC 容器的初始化和依赖注入
##### Bean 的初始化分为三步
- Resource 定位，根据开发者配置进行资源定位。
- BeanDefinition 载入，Spring 根据配置获取对应的 POJO，生成实例
- BeanDefinition 注册，将前一步载入的 POJO 注册到 Spring IOC 容器中，之后开发人员就可以通过描述从 Spring IOC 容器中得到对应的 bean。

##### lazy-init 懒加载
默认为 false，Spring IOC 会自动初始化 Bean，如果设置为 true，只有使用 Spring IOC 容器使用 getBean 方法获取时，才会进行初始化，完成注入。

#### 9.3.3 Spring Bean 的生存周期
- 初始化，可以在 bean 的 init-method 中设置对应的 bean 函数
- 如果 Bean 实现了 BeanNameAware 的 setBeanName 方法，那么会调用这个方法
- 如果 Bean 实现了 BeanFactoryAware 的 setBeanFatory 方法，会调用这个方法
- 如果 Bean 实现了 ApplicationContextAware 的 setApplicationContext 方法，其 Spring IOC 容器是一个 ApplicationContext 接口的实现类，那么会调用这个方法
	- 如果不是实现的 ApplicationContext 接口，那么不会调用
- 如果 Bean 实现了接口 BeanPostProcessor 的 postProcessBeforeInitialization 方法，那么会调用这个方法。
- 如果 Bean 实现了接口 BeanFactoryPostProcess 的 afterPropertiesSet 方法，那么会调用这个方法
- 如果 Bean 定义了初始化方法，会调用
- 如果实现了接口 BeanPostProcessor 的 PostProcessAfterInitiallization 方法，完成这些调用，这时候 Bean 就完成了初始化，使用者可以在 IOC 容器中开始使用。
- 关闭后，如果 Bean 实现了 DisposableBean 的 destroy 方法，那么调用
- 如果定义了自定义销毁方法，那么就会调用它

##### 注意
接口 BeanPostProcessor 针对所有 Bean，接口 DisposableBean 针对 Spring IoC 容器本身。

实现书中案例之后，运行的输出为
```java
对象source 开始实例化
【DisposableBeanimpl 】对象disposableBean 开始实例化
【DisposableBeanimpl 】对象disposableBean 实例化完成
【Source 】对象source 开始实例化
【Source 】对象source 实例化完成
【JuiceMaker2 】调用BeanNameAware 接口的setBeanName 方法
【JuiceMaker2 】调用BeanFactoryAware 接口的setBeanFactory 方法
【JuiceMaker2 】调用ApplicationContextAware 接口的setApplicationContext 方法
【JuiceMaker2 】对象juiceMaker2 开始实例化
【JuiceMaker2 】调用InitializingBean 接口的afterPropertiesSet 方法
【JuiceMaker2 】执行自定义初始化方法
【JuiceMaker2 】对象juiceMaker2 实例化完成
这是一杯由贡茶饮品店，提供的大杯少糖橙汁
【JuiceMaker2 】执行自定义销毁方法
调用接口DisposableBean 的destroy 方法
```

其中
```java
【DisposableBeanimpl 】对象disposableBean 开始实例化
【DisposableBeanimpl 】对象disposableBean 实例化完成
【Source 】对象source 开始实例化
【Source 】对象source 实例化完成
```
是 BeanPostProcessor 的输出，可以看出每个 Bean 的实例化开始和完成都会经过其中的两个函数。
其中
```java
调用接口DisposableBean 的destroy 方法
```
是 DisposableBean 针对 Spring IoC 容器，当 IoC 容器中所有 bean 销毁之后，才会调用其中的 destroy 方法。了解了 Bean 的生命周期之后，就可以利用 Bean 的生命周期来完成一些需要自定义的初始化和销毁 Bean 的行为了。

## 第十章 装配 Spring Bean
- 三种依赖注入方式
	- 构造器注入， `constructor-arg` 子标签
	- setter 注入, `property` 子标签
	- 接口注入

### 10.2 装配 Bean
使用 ApplicationContext 接口的具体实现类来将 Bean 装配到 Spring IoC 容器中。
- 在 XML 中显式配置
- 在 Java 的接口和类中实现配置，次优方案，可以避免 XML 配置的泛滥
- 隐式 Bean 的发现机制和自动装配机制，一般优先选择该方式，好处是简单

### 通过 XML 配置装配 Bean
- 装配简易值
- 装配集合
- 命名空间装配, 构造器参数使用 `c:_0` 和 `c:_1` 简化 `constructor-arg` 子标签，使用 `p:id` 简化 `property` 子标签

### 通过注解装配 Bean
- `@Component` 装配 Bean，直接用 Value 来赋值
- `@Autowired` 默认按照类型来查找


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
配置 Mybatis-Spring 项目的步骤
- 配置数据源，即 dataSource
- 配置 SqlSessionFactory
- 可以选择的配置有 SQLSessionTemplate，在同时配置 SqlSessionTemplate 和 SqlSessionFactory 的情况下，优先采用 SqlSessionTemplate。
- 配置 Mapper
- 事务管理

### 12.4.1 配置 SqlSessionFactoryBean
Mybatis-Spring 项目中提供了 SQLSessionFactoryBean 去支持 SqlSessionFactory。SqlFactoryBean 中的属性对应于 Mybatis 的 xml 配置文件。

### 12.4.2 SqlSessionTemplate 组件
- 是线程安全的类，也就是确保每个线程使用的 SqlSession 唯一且不互相冲突。
- 每当运行一个 SqlSessionTemplate 时，就会获取一个新的 SQLSession，每个方法都是独立的 SqlSession，这样也就保证了线程安全。

### 12.4.3 配置MapperFactoryBean
MapperFactoryBean 作为中介来实现编者想要的 Mapper，将 Mapper.xml 映射为实现了对应 Mapper 接口的实体类 Bean。将 mapper 接口加入 Bean 中。

### 12.4.4 配置 MapperScannerConfigurer
通过扫描的形式进行配置 Mapper，将所有的包下的 mapper 接口加入 Bean。可以配置以下属性
- basePackage，目标包名
- SqlSessionFactory
- annotationClass，被指定注标修饰的才扫描成为 Mapper，一般使用 Repository 注解
- markerlnterface，指定实现了什么接口就认为是 Mapper，这样要求额外定义一个标志接口，所有的 Mapper 都实现它，它可以为空。一般不推荐使用。

## 第 十三 章 深入 Spring 数据库事务管理
- 为何会有事务？
主要为了数据访问安全和数据一致性。数据库中已经有了四种隔离级别，分别为未提交读，已提交读，可重复读，串行读。后三种隔离级别依次能够克服脏读，不可重复读，幻读等问题。 spring 为了支持数据库事务提供了传播行为。

### 13.1 Spring 数据库事务管理器的设计
支持事务的是 org.springframework.transaction.support.TransactionTemplate 模板。
事务的过程为
- 事务的创建、提交和回滚是通过 PlatformTransactionManager 接口来完成的
- 当事务产生异常时会回滚事务，在默认的实现中所有的异常都会回滚。可以通过配置来修改在某些异常发生时回滚或者是不回滚事务。
- 当无异常时，会提交事务。

PlatformTransactionManager 接口的源码如下
```java
public interface PlatformTransactionManager{
  TransactionStatus getTransaction(TransanctionDefinition definition) throws TransactionException;
  void commit(TransactionStatus status) throws TransactionException;
  void rollback(TransactionStatus status) throws TransactionException;
}
```

### 13.1 配置事务管理器
配置 transactionManager bean，将 dataSource 配置 bean 属性。这样表明已经将数据库事务委托给事务管理器 transactionManager 管理了。

### 13.3 声明式事务
在操作数据库的代码上都可以使用注解声明事务

### 13.5 传播行为
spring 事务定义了七种传播行为，其中最常用的是三个有
- REQUIRES，当方法调用时，如果有没有事务，则创建，如果有，那么沿用，spring 默认的传播行为
- REQUIRES_NEW，无论是否存在当前事务，都创建新事务。
- NESTED，嵌套事务，保存点，如果发生异常，不全部回滚，只回滚到保存点上。如果数据库不支持保存点，那么会退化成 REQUIRES_NEW。

## 第 十七 章 redis
- redis 的作用
	- 速度快，用作缓存
	- 数据一致性和访问控制，可以用 redis 实现分布式锁
- redis 六种数据结构
	- string，可以保存字符串，正数和浮点数。
	- list，链表，每个节点包含一个字符串
	- hash，类似于 java 中的 Map
	- set，无序，每个元素唯一，可以做交集，并集和差集操作。
	- zset，有序集合
	- HyperLogLog，基数，计算重复的值，确定存储的数量

- redis 在 spring 中使用
	- 对象需要序列化，将 Java 对象存入 Redis 时将对象序列化，从 Redis 中取出时将序列化字符串反序列化成对象

- redis 优缺点
	- 优点
		- 访问速度快
	- 缺点
        - 机器故障，在内存中的数据会丢失
        - 事务支持简单

## 第 十九 章 redis 的事务
- 事务
	- 开始
	- 提交
	- 回滚
		- 命令不正确，全部回滚
		- 数据格式不正确，不会回滚，这点和数据库很不同
- 监控事务
	- watch 命令可以监视一个或多个键值，一旦其中一个键被修改，之后的事务不会执行，监控会持续到 EXEC 提交事务为止或者是放弃事务为止。
	- 举例
	```java
        WATCH mykey
        val = GET mykey
        val = val + 1
        MULTI
        SET mykey $val  // 如果 mykey 被修改，那么这个事务不会执行
        EXEC
    ```
- 流水线协议
	- 事务操作，事务子操作中间间隔时间过长
	- 解决方案，流水线协议
- 发布订阅
	- SUBSCRIBE 创建一个管道
	- publish 消息
- 超时命令
	- 超时回收方法
		- 定时回收
		- 惰性回收
			- 下一次调用 get 的时候开始回收

## 第 二十 章 Redis 的配置
- 备份
	- 快照，将某一个时刻内存中的数据备份到磁盘上
    - AOF 追加
        - 将数据库操作记录追加到 AOF 中
- Redis 内存回收策略
	- volatile-lru: 对超时的键使用 LRU，时间内最少使用
	- allkeys-lru: 对所有的键值采用 LRU
	- volatile-random: 对超时键值采用随机淘汰
	- allkeys-random: 对所有的键都采用随机淘汰
	- volatile-rtt：删除存在时间最短的键值
	- noeviction：不使用删除策略，只是当内存满的时候，只能调用 get 方法，不能使用写操作，否则报错
		- redis 默认使用该策略
- 复制
	- 主从复制
		- 读写分离，只往主数据服务器写，从从服务器上读，主服务器写完，然后将更改同步到从服务器。
			- 前提是，读操作远远大于写操作
	- 主从同步
		- 主服务器写数据
		- 从服务器读数据
		- 主服务器写完数据，同步到从服务器
		- 通过均衡算法，将读操作分散到对应的从服务器上
		- 容错性，从服务器故障，不影响，主服务器故障，剩余服务器选举出一个主服务器
	- 主服务器故障后的主从切换的自动模式——哨兵模式
		- 哨兵的作用
			- 通过发送命令，监控多个 redis 的运行状况
			- 检测到 master 宕机，然后从 slave 升级成 master
		- 多个哨兵互相监督
			- 当一个哨兵发现主机宕机之后，其他哨兵参与投票，确定主机宕机，达到一定数量之后，就重新选举 master。
			- 主机切换成功之后，通过发布订阅的方式告知其他主机。


## 第 二十二 章 高并发业务
### 高并发系统
- 结构
	- 防火墙
	- 负载均衡
	- NoSql 服务器
	- Web服务器
	- CDN
	- 数据库
- 负载均衡器
	- 区别无效 IP，无效请求
		- 应对无效请求的方法，加验证码或发送认证短信，对验证码和单位时间单个账号请求量进行判断，识别无效用户或者是无效 ip
	- 提供路由算法，也应该提供负载均衡算法
	- 限流，对于请求过多的时刻，可以提示用户系统繁忙
- 系统设计
	- 按业务分（水平分法），每种业务单独放置在一个服务器上，互相之间通过 RPC 调用，常见的 RPC 有 Dubbo、Thrift 和 Hessian 等。
	- 按子系统分（垂直分法），将相同的逻辑在多个子服务器上实现，当请求来时，分发下去。
	- 水平和垂直结合
- 数据库设计
	- 分表
	- 分库
		- 事务一致性
- 动静分离技术
	- 静态数据 `—>` CDN
	- 动态数据 `->` 动态服务器

### 案例 抢红包（锁和高并发）
假设有 20 万元的红包，可以拆分成 2 万个可抢的小红包。假设每个红包是 10 元，供给网络同时存在 3 万会员在线抢夺。在一个瞬间产生很高的并发，除了保证数据的一致性还要尽可能保证系统的性能。
#### 不加锁版本
不加锁版本会出现超发现象。[代码实现](https://github.com/shihunyewu/grab-red-packet)。




##### 参考书
`<<Java EE 互联网轻量级框架开发 SSM框架>>`
