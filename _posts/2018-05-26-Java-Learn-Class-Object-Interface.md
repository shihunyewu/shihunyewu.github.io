---
layout:     post
title:      "Java-Learn"
subtitle:   " \"Java 学习中 —— 类与对象、继承、接口、内部类\""
date:       2018-05-26 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - Java
---

## 4 章 对象与类
* 封装的优点
	* 更改器可以执行错误检查，防止出错
	* 注意：不要返回引用可变对象的访问器方法，这样会破坏类成员的封装性，因为这意味着返回了一个引用
		* 如果想要返回一个可变数据域的拷贝，应该使用 clone

* 静态方法之于工厂方法
	* 使用静态工厂方法产生不同风格的格式对象

* 方法参数的使用情况
	* 方法不能修改基本数据类型的参数（数值型和布尔型）
	* 方法可以改变对象的状态，比如说改变对象的属性值
	* 方法不能让对象参数引用一个新的对象，即该对象引用本身的值不能被改变
* java 中的函数参数没有引用传递

* java 中可以显式域初始化，即定义类成员变量时赋值， c++ 中不能直接初始化类的实例域。

* 对象析构和 finalize 方法
	* 类的 finalize方法将在垃圾回收器清除对象之前调用。但是因为 java 有自动的垃圾回收器，因此该很难知道这个方法什么时候会被调用。对于某些资源，非必须的情况下不应该放在该函数中回收。

* 包
	* import 语句能够导入包中特定的类，使用 .* 可以导入包中所有的公有类
	* 静态导入
		* import static java.lang.System.*;  可以使用 System 类的静态方法和静态域
		* import static java.lang.System.out;	可以导入特定的静态方法或域
		* 没有 static 关键字会报错

* 包作用域
	* 如果没有指定 public 还是 private， 这个部分可以被同一包内的所有方法访问

* javadoc  api 文档自动生成器
	*  @关键字
	*  添加包注释的方法

* 类设计技巧
	* 保证数据私有性
	* 对数据初始化
	* 不要在类中用过多的基本类型
		* 应该将多个相关的基本类型使用一个类来代替
	* 并非所有的域都需要访问器和更改器
	* 将职责较多的类进行分解
		* 尽可能地让类的智能单一
	* 类名和防范要能够体现它们的职责

## 5 章 继承

* super 和 this 并非类似的概念，super 不是一个对象的引用，不能将 super 赋给另一个对象变量，它只是一个指示编译器调用超类的特殊关键字。

* this 有两个用途
	* 引用隐式参数
	* 调用该类的其他的构造器
* super 两个用途
	* 调用超类的方法
	* 调用超类的构造器

* Java 中不需要将方法声明为虚拟方法，动态绑定是默认的处理方式，如果不想让一个方法具有虚拟特征，可以将其标记为 final

* 继承关系，"is-a" 规则
	* 举例说明，Manager 类是 Employee 类的子类，因为每个经理都是雇员

* 调用对象方法的执行过程
	* 查看对象的声明类型和方法名，编译器获得所有可能被调用的候选方法
	* 查看所有候选调用方法的参数类型，查找和参数类型匹配的方法，这被称为重载解析
	* 查看方法的修饰符，编译器将指导应该调用哪个方法，这被称为静态绑定
	* 动态绑定调用方法，虚拟机需要调用与 x 所引用对象的实际类型最合适的那个类的方法
	* 注：实际上虚拟机预先为每个类创建了一个方法表，其中列出了所有方法的签名和实际调用的方法，节省搜索的时间。

* 在覆盖一个方法的时候，子类方法不能低于超类方法的可见性，特别是超类方法是 public，子类方法一定要声明为 public，否则，调用该方法是，实际上会调用父类方法

* 阻止继承：final 类和方法
	* 阻止类被继承： final class Manager{...}
	* 阻止方法被继承： public final String getName(){...}

* 对象之间的强制转换
	* 预防在继承链上进行向下的类型转换，可以使用 instanceof 运算符来避免，比如
		```java
        if(staff[1] instanceof Manager){ // 先判断是否是 Manager 类的对象
        	boss = (Manager)staff[1];
        }
        ```
	* 应该尽量避免对象之间进行强制转换，尽量使用多态性的动态绑定机制处理

* 抽象类
	* 抽象类和抽象方法的声明
		```java
        abstract class Person{
        	private String name;
            public abstract String getName(){
            	return name;
            }
        }
        ```
	* 抽象类中可以有具体的数据和具体的方法
	* 含有抽象方法的类一定是抽象类，抽象类也可以不含抽象方法，但是这样就失去了将其定义为抽象类的意义。
	* 抽象类不能有实例化对象
* java 用于控制可见性的 4 个访问修饰符
	* private：仅对本类可见
	* public： 对所有类可见
	* protected：对本包和所有子类可见
	* 默认（无修饰符）：对本包可见

* equals 方法
	* Object 类中的 equals方法， 用于检测一个对象是否等于另外一个对象，只是判断两个对象是否具有相同的引用。但是这样的判等没有什么意义。大多数的 equals 方法都需要重写，比如 String类的 equals方法
	* 重写 equals 时，首先调用超类的 equals，再比较子类中的实例域

* equals 方法的相等测试与继承
	* java 语言规范规定 equals 需要具有的规范
		* 自反性，对称性，传递性，一致性，对 null 应返回 false
	* 子类和父类之间的 equals 问题
		* 使用 instanceof 做类相等测试时，会造成对称性方面的问题
	* JDK 中的 equals 方法实现多种多样

* hashCode 方法
	* Object 默认的 hashCode 是返回对象存储地址，两个对象一定不同
	* String 的散列码是由内容导出的，因此两个内容相同的字符串的 hashCode 会是相同的
	* 注意：
		* 如果重新定义了 equals 方法，需要重新定义 hashCode 方法，以便在使用散列表中不会出现 key 相同的情况
* toString 方法
	* Object 类的 toString 方法，打印输出对象所属的类名和散列码
	* 数组的 toString 方法
		* 一维数组打印: Arrays.toString(array);
		* 多维数组打印: Arrays.deepToString(array);

* 对象包装器
	* 基本类型的包装类有： Integer, Long, Float, Double, Short, Byte, Charater, Void, Boolean
	* 包装器中包含了很多有用的静态方法，比如 Integer.parseInt(s)，将字符串转换成数字

* 自动装箱
	* list.add(3); 相当于自动调用 list.add(Integer.valueOf(3));

* 自动拆箱
	* int n = list.get(i); 相当于调用 list.get(i).intValue();

* 包装器之间的判等是有条件的
	* 包装器根本上来说是对象，直接 == 实际上是比较的对象在内存中放置的地址
		* 但是包装器出于优化的目的，在内存中提前放置了常用值，比如 int 类型的包装器维护了一个 -128~127 的数组，Integer 的值在 -128~127 之间时，会被判做相等。但是超过这个范围就会被判做不等。
	* 包装器类型之间的比较应该使用 equals

* 参数数量可变的方法
	* Object[] args
	```java
    static void multiArg(String ...strings) { // 特别的地方就是 ... ， 实际上 strings 是一个 String 数组
		for (String string : strings) {
			System.out.println(string);
		}
	}
    ... 调用方法
    multiArg("wo", "hello");
    ```

* 枚举类
	* 枚举类
		* 简单定义
		```java
        public enum Size{SMAILL, MEDIUM, LARGE, EXTRA_LARGE};
        ```
        此处声明定义的类型是一个类，有 4 个实例。
       	* 使用
       	```java
        	Size size = Size.MEDIUM;
            if(size == Size.MEDIUM) {
                System.out.println("M");
            }
        ```

* 继承设计的技巧
	* 将公共操作和域放在超类
	* 不要使用受保护域
		* 子类在继承过程中，会破坏封装性
		* 同一个包中的所有子类都可以访问protected域
	* 使用继承实现 "is a" 关系
	* 除非所有继承的方法都有意义，否则不要使用继承，应该使用接口实现？
	* 在覆盖方法时，方法实现要和预期的行为一致
	* 使用多态，而非使用类型信息，即用 intanseof 来判断类型
	* 不要过多使用反射
		* 编译器很难帮助人们发现其中的错误，只有运行时才会发现错误并导致异常。

## 第 6 章 接口和内部类