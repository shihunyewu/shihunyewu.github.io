---
layout:     post
title:      "spring core code"
subtitle:   "spring 源码跟读"
date:       2019-05-17 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - Java
    - Spring
---

## 第 1 章 Spring 整体结构
Spring 的整体架构
- Core Container，核心容器
	- Core，提供 IoC 和依赖注入
	- Bean，提供 IoC 和依赖注入， BeanFactory
	- Context，框架式的对象访问方法
	- Expression Language，即 SpringEL
- Data Access/ Integration
	- JDBC，提供了一个 JDBC 抽象层，主要利用 AOP 实现
	- ORM，对象-关系映射 API
		- 支持事务，支持 Object/XML 映射
- Web
	- Web 模块：提供了基础的面向 Web 的集成特性
	- Web-Servlet，即 Spring MVC
	- Web-Struct
	- Web-Porlet
- AOP
	- 面向切面的标准实现
- Test
	- 支持 Junit 和 TestNG 对 spring 组件测试

## 第 2 章 容器的基本实现
Bean 被维护在 Spring IOC 容器中。Spring 入门示例是用 XmlBeanFactory 加载 Xml 文件然后从容器中获取 bean。
##### 核心类介绍
1. DefaultListableBeanFactory，最核心的加载类
	- 包含有 BeanDefinitionReader 成员，该类主要用来读取配置类
	- XmlBeanFactory 继承了该类，并实现了自定义的 XML 读取器 XmlBeanDefinitionReader。
##### 使用 XmlBeanFactory 来加载 xml 文件的过程
1. 获取对 XML 文件的验证模型
2. 加载 XML 文件，并得到对应的 Document。
3. 根据返回的 Document 注册 Bean 信息。

###### 获取 XML 的验证模式
- [DTD 和 XSD](https://blog.csdn.net/ningguixin/article/details/8171581)
	- DTD 和 XSD 都是 xml 的规范，但是 XSD 要比 dtd 好，spring 中采用的就是 xsd
###### 获取 Document
通过 SAX 解析 XML。

## 第 3 章 默认标签的解析
默认标签分为四种
1. import，引入其他 spring 配置文件
2. alias，定义别名
3. bean，基本的 bean 定义
4. beans，包含了很多 bean 定义的标签

##### bean 标签的解析及注册


## 第 5 章 Bean 的加载
##### spring 如何解决循环依赖
spring 中将循环依赖的处理分成了 3 种情况
1. 构造器循环依赖，这是无法解决的
	- spring 会抛出 BeanCurrentlyInCreationException
2. setter 循环依赖
	- 通过 spring 容器提前暴露刚完成构造器注入但未完成其他
3. prototype 范围的依赖处理
	- 对于 “prototype”作用域 bean，Spring 容器无法完成依赖注入，因为 Spring 容器不进行缓存 “prototype” 作用域的 bean，因此无法提前暴露一个创建中的 bean











#### 参考书籍

`<<Spring 源码深度解析>>`