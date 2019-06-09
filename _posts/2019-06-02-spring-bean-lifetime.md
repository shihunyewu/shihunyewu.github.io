---
layout:     post
title:      "spring bean lifetime"
subtitle:   " \" spring bean 的生命周期 \""
date:       2019-06-02 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - spring
---
## spring bean 的生命周期

### 一. spring bean 的完整的生命周期
如下图所示
![spring 的生命周期](/img/blog_img/spring-bean-lifetime.png)

### 二、各种接口方法分类
Bean的完整生命周期经历了各种方法调用，这些方法可以划分为以下几类：
#### 1. Bean 自身的方法
这个包括了 Bean 本身调用的方法和通过配置文件中<bean>的 init-method 和 destroy-method 指定的方法
#### 2. Bean 级生命周期接口方法
- BeanNameAware，可以获取 bean，可以让 bean 对自己的 bean name 有知觉
- BeanFactoryAware，可以获取 bean factory 的信息，让 bean 对 bean factory 有知觉
- InitializingBean，内含一个方法为 afterPropertiesSet
	- spring 提供了 init-method 用来在实例化 bean 之后对 bean 做操作，其可以和 afterPropertiesSet 一起使用，两者功能相似
	- afterPropertiesSet 比使用反射调用 init-method 方法快一点，因为直接调用 afterPropertiesSet 方法即可
	- afterPropertiesSet 会增加业务代码和 spring 框架的耦合度
- DiposableBean，销毁 bean 的方法，和 InitializingBean 相似，也有 destroy-method 和 其相对应

对于 BeanName 和 BeanFactoryName 实际上平常更多地是将 bean 的信息打印到日志中。在日常使用过程中应该极力避免使用这两个接口，因为我们使用 spring 的目的是解耦，如果实现这些接口之后明显就让业务代码和 spring 框架耦合在了一起。

#### 3. 容器级生命周期接口方法
这个包括了 InstantiationAwareBeanPostProcessor 和 BeanPostProcessor 这两个接口实现，一般称它们的实现类为"后处理器"。
- InstantiationAwareBeanPostProcessor
- BeanPostProcessor，最顶层接口，内含两个方法 postProcessAfterInitialization 和 postProcessBeforeInitialization

#### 总结
1. Spring 内部对 bean 的构造已经形成了一套体系。如果我们想修改这套体系，只能使用 Spring 提供的接口去处理。这样做的好处是遵循设计模式的开闭原则，对扩展开放，对修改关闭。 我们只需要实现接口进行扩展即可，不需要修改内部的源码。

#### 参考
1. [spring life cycle](https://www.journaldev.com/2637/spring-bean-life-cycle)
2. [Spring Bean的生命周期（非常详细）](https://www.cnblogs.com/zrtqsk/p/3735273.html)
3. [BeanNameAware and BeanFactoryAware Interfaces in Spring](https://www.baeldung.com/spring-bean-name-factory-aware)
4. [InitializingBean的作用](https://blog.csdn.net/maclaren001/article/details/37039749)
5. [Spring内部的BeanPostProcessor接口总结](https://fangjian0423.github.io/2017/06/20/spring-bean-post-processor/)

