---
layout:     post
title:      "Java-Learn"
subtitle:   " \"Java 学习中 —— 异常、断言、日志和调试\""
date:       2018-05-29 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - Java

## 第 11 章 异常、断言、日志和调试
[](何种错误使用返回值，何种错误应该捕获，何种错误应该直接作为返回值？)
如果由于程序出现了错误而使得某些操作没有完成，程序应该：
* 返回到一种安全状态，并能够让用户执行一些其他的命令
* 允许用户保存所有操作的结果，并以适当的方式终止程序


### 11.1 异常
#### 11.1.1 异常分类
* RuntimeException，由程序错误导致的异常属于 RuntimeException
* 其他异常，程序本身没有问题，由文件、设备和用户输入等其他不可控因素引起的异常。

派生于 RuntimeException 的异常主要有
* 错误的类型转换
* 数组访问越界
* 访问空指针

出现 RuntimeException 异常，那么就一定是你的问题。比如说我们应该通过检测数组下标是否越界来避免数组访问越界异常；应该在使用变量之前就检测该变量是否为空来避免访问空指针的异常。

#### 11.1.2 声明已检查异常
这一点通常大多数的 IDE 都能够做到自动检测方法中已检查的异常，然后强制编程者在方法头部用```throw XXException```标出。


**注意**，当子类的覆盖了父类的一个方法，子类方法中声明的已检查异常不能比超类方法中声明的异常更通用。比如说超类抛出 IOException，子类只能抛出 IOException 的子类；另外，如果超类方法没有抛出任何已检查异常，子类也不能抛出任何已检查异常

#### 11.1.3 抛出异常
关键是找到一个合适的异常类，问题描述得越精确越好。如果没有对应的异常类，可以自己创建异常类。

### 11.2 捕获异常
#### 11.2.1 再次抛出异常
* 关注点不同再次抛出的异常不同
现在的情景是你开发了一个供其他成员使用的子系统，那么用于表示系统故障的异常类型可能产生多种解释，但是该子系统的用户只关注这个子系统是否有问题，因此可以将子系统中的 IOException 这样的异常捕获后，重新封装一个子系统有关的异常。下面提供一个 Servlet 的案例
```java
try{
  access the database
}catch(SQLException e){
	throw new ServletException("database error：" + e.getMessage());
}
```
虽然是 SQLException 异常，但是封装成了 ServletException 异常，告诉用户这是 Servlet 这个子系统的内部错误。

**启示**
我们在开发子系统的时候可以用这种方式来让用户区分他们自己编程造成的异常和子系统引起的异常。

#### finally 子句
**finally 子句无论如何都会执行**
即使是 try 子句中存在 return 语句，finally 也会执行，而且当 finally 也包含有 return 语句的时候，会覆盖掉 try 中的 return。

**建议**

独立使用```try/catch``` 和 ```try/finally```语句块，例如
```java
try{
	try{
    	code
    }finally{
    	in.close();
    }
}
catch(IOException e){
	show error
}
```
这样写一方面逻辑清晰，即 ```in.close()``` 语句紧跟文件操作语句，让人很容易注意到我们做了文件关闭。另一方面，finally 子句关闭文件的时候也可能出现错误，catch 语句可以同时捕获 finally 中的异常。

#### 11.2.4 带资源的 try 语句
```java
try(Resource res = ...){
	work with res
}
```
try 子句执行完成后，会自动调用 res.close();这样不必再单独写 finally 子句来关闭资源了。close() 会将异常交给 try 语句。
#### 11.2.5 堆栈跟踪
在调试的时候可以使用

### 11.3 使用异常机制的技巧
