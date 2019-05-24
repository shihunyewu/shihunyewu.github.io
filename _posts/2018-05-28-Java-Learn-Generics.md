---
layout:     post
title:      "Java-Learn"
subtitle:   " \"Java 学习中 —— 泛型\""
date:       2018-05-28 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - Java
---

## 12 章 泛型程序设计
### 12.1 为何要使用泛型
有了泛型之后，相同的操作只要定义一份代码就可以适配各种类型，节省了代码量，避免了编写重复代码。

### 12.4 类型变量的限定
```java
	// 此处将 T 限制为实现了 Comparable 接口的类
    public static <T extends Comparable> T min(T[] a){...}
```

* extends 绑定接口或者是类，这里统一使用 extends 来表示
* 可以 T 做多个类型的限定

```java
	// 注意，这里也只能继承一个类，需要其他的类型只能是接口
    public static <T extends Comparable & Serializable> void func(T[] a){....}
```

### 12.5 泛型代码和虚拟机
* 虚拟机没有泛型类型对象，所有对象都属于普通类，在替换泛型的时候，替换成对应的类型，如果没有限定，使用 Object 替换。
	* 对于` <T extends Serialiable & Comparable > ` 原始类型使用 Serialiable 替换 T，在必要的地方，将插入 Comparable 强制类型转换。因此为了提高编译效率，应该将没有方法的接口放在边界列表的末尾。

#### 12.5.1 翻译泛型表达式
* [参考](https://blog.csdn.net/luotuomianyang/article/details/51037739)
```java
public class DateInterval extends Pair<Date> {...} // 泛型类型擦除的时候，会直接无视 <Date>，使用原始的 Pair 作为 DateInterval 的父类
```
* 为了保持多态的特性，虚拟机会再产生一个桥方法
```java
public void setSecond(Object second){
    this.setSecond((java.util.Date) second );
}
```

### 12.6 约束与局限性
- 不能用基本类型实例化类型参数
```java
Pair<double> pair = ...// Error
Pair<Double> pair = ...// Right，只能用对象
```
- 运行时类型查询只适用于原始类型
```java
if( a instanceof Pair<String>) // Error
```
只能检测一个对象是否是 Pair 类型。
使用 getClass 方法总是返回原始类型 Pair.class。

- 不能创建参数化类型的数组
- Varargs 警告，方法允许存在参数个数可变的泛型。
- 不能实例化类型变量
```java
new T() // Error
```
- 泛型类的静态上下文中类型变量无效，[参考](https://blog.csdn.net/TimHeath/article/details/53561802)
- 不能抛出或捕获泛型类的实例，不能抛出也不能捕获泛型对象，泛型类不也不能扩展 Throwable。
- 擦除后的冲突，[参考](https://blog.csdn.net/TimHeath/article/details/53561802)

### 12.7 泛型类型的继承规则
Employee 和 Manager 是父类和子类的关系，但是 ``` Pair<Manager> ```和 ```Pair<Employee>``` 二者不是父类和子类的关系。

### 12.8 通配符类型
#### 出现原因
通配符类型主要是为了解决泛化问题，因为泛型类型的继承规则``` Pair<Employee> ``` 和 ``` Pair<Manager>```不是父类和子类的关系。如果函数的参数为 ```Pair<Employee> ```，这时却不能传 ``` Pair<Manage>```这个对象。因此就出现了通配符类型。
#### 具体含义
```Pair<? extends Employee>``` 表示此处的泛型对象只能是 Employee 类的子类，用这个类型作为函数参数的时候就可以传```Pair<Manager>``` 对象了。
#### 可能存在的问题
如果使用```Pair<? extends Employee>``` 来引用 ```Pair<Manager>```对象，是否会使用其他的 Employee 子类来替代被引用了的 ```Pair<Manager>```中的 Manager 对象？需要分析一下，现在为了贴合这个例子，我们假设```Pair<Manager>```中因为被```Pair<? extends Employee>```引用了之后，原来的
```java
void setFirst( Employee)
```
变成了这样一个函数
```java
void setFirst(? extends Employee)
```
而```?```在非泛型结构中是没有意义的，不能像是在```Pair<? extends Empolyee>```作为通配符使用，没有任何对象是不定类型的```? ```。


