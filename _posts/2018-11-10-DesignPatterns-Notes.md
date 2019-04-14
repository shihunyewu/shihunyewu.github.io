---
layout:     post
title:      "DesignPatterns-Note"
subtitle:   " \"常用设计模式\""
date:       2018-11-15 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - DesignPatterns
---
> 设计模式离我们并不遥远，JDK 实现以及日常使用的框架中都有它的身影

## 单例模式
[参考](http://www.runoob.com/design-pattern/singleton-pattern.html)
### 主要特点
系统中只需要一个实例即可，不需要创建很多的实例。
### 主要注意问题
* 懒汉模式
* 饿汉模式
* 多线程下的加锁，创建单例的时候需要加锁，防止在没有创建实例时，防止两个线程同时调用获取单例方法，这时候可能会创建两个单例。

### 具体应用场景
#### spring
spring IoC 容器默认是采用单例模式来管理 Bean 的，使用单例模式时，IoC 容器中存放的 Bean 对象只有一个实例。

## 观察者模式
### 主要特点
角色分为被观察者和观察者两种角色。被观察者发生特定事件时会通知所有注册在案的观察者，观察者们收到通知后再采取进一步操作。
### 具体的应用场景
#### 网络通信
A 线程运行 UI 界面，B 线程负责网络通信，B 线程网络通信完成后，通知 A 相关函数改变 UI，展示相关的数据。
#### UI 设计
java 中的 UI 库 Swing 也是类似，用 Listener 来作为观察者， UI 有动作之后会通知 Listener 相关的动作方法。

## 装饰器模式
允许向一个现有对象添加新的功能，同时又不改变原有对象的结构。主要分为装饰器类，被装饰类。装饰器类和被装饰类都引用自同一个接口，这个是为了保证装饰器类能够重写被装饰类的方法。另外装饰器类引用被装饰类，保证在重写的方法中能够再调用被装饰类的方法，保证被装饰类基本功能的保留。

### 意图
动态地给一个对象添加一些额外的职责，这些功能可以动态添加，动态删除。和继承相比装饰器模式更加灵活。继承只能是静态的，用户不能控制增加行为的方式和实际。装饰模式对用户以透明的方式给一个对象添加更多责任，装饰模式可以在不需要创建更多子类的情况下，将对象的情况加以扩展。
### 优缺点
[参考](https://design-patterns.readthedocs.io/zh_CN/latest/structural_patterns/decorator.html)
### 具体的应用场景
#### Java IO 设计
Java I/O 使用了装饰器模式来实现，以 InputStream 为例。
- InputStream 是抽象组件，是装饰器和被装饰者的统一接口。
- FileInputStream 是 InputStream 的子类，属于具体组件，就是被装饰者，提供了字节流的输入操作。
- FileInputStream 抽象装饰器，相关的具体的装饰器需要继承实现该抽象装饰器。
- BufferedInputStream 具体的装饰器，具体的装饰者用于装饰组件，为组件提供额外的功能。

![参照](https://raw.githubusercontent.com/CyC2018/CS-Notes/master/docs/notes/pics/DP-Decorator-java.io.png)

## 适配器模式
使用既有类来实现另一个接口，实现方式可以是继承接口和既有类，或者是继承接口，使用既有类作为成员，在接口方法中借用既有类的方法。
### 具体应用场景
- HashSet 底层使用 HashMap 实现

```java
    /**
     * Constructs a new, empty set; the backing <tt>HashMap</tt> instance has
     * default initial capacity (16) and load factor (0.75).
     */
    public HashSet() {
        map = new HashMap<>();
    }
```
- TreeSet 底层使用 TreeSet 实现
- Stack 底层使用 Vector 实现

