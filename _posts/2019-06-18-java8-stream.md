---
layout:     post
title:      "java8 stream"
subtitle:   " java8 新特性之 stream"
date:       2019-06-018 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - java
    - stream
---
> java8 stream 和 lambda 表达式可以让代码更加简洁。

主要关注 lambda 表达式和 java8 stream。

### lambda 表达式
lambda 可以省略很多的模板代码，一个 lambda 表达式作为接口的一个实现，这个接口需要具备的条件是，只有一个函数，除此之外，lambda 表达式的参数个数和类型要和接口中的函数的相同。举个例子：

这是接口的定义，其中定义了一个方法，其中入参有两个 a 和 b，返回值为 int
```java
interface MathOperation {
	int operation(int a, int b);
}
```
在 main 方法中定义了一个 lambda 表达式，其中 lambda 表达式的参数和返回值和接口中的函数对应
```java
public static void main(String[] arg) {
    MathOperation add = (a, b)->a + b; // 如果只有一条语句，可以省略 {}
    int result = add.operation(10, 10);
    System.out.println(result);
}
```

比较常见的就是在排序时实现 Comparator 接口的比较方法
```java
String[] ss = {"144", "12", "123"};
Arrays.sort(ss, (o1, o2)->o2.length() - o1.length());
System.out.println(Arrays.toString(ss));
```


#### 参考
1. [【译】Java 8的新特性—终极版](https://www.jianshu.com/p/5b800057f2d8)
2. [Stream operations and pipelines](https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html#StreamOps)