---
layout:     post
title:      "regular-expression-matching"
subtitle:   "Java编程的助手 - guava"
date:       2019-05-15 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - Java
---
> 使用像 Guava 这样的库远远好于自己写

Guava 是 google 开发的一个 java 集合库，这些类库包含字符串、集合、并发、I/O 和 反射工具。Guava 类似于一个语法糖，让开发者的开发更便捷也更不容易在某些细节上出错。
由于 Guava 是第三方库，因此使用之前需要下载。

## 第 2 章，基础的 Guava
####  Joiner 类
Joiner 主要用来将列表中的字符串按照分割符连接在一起。
```java
List<String> stringList = new ArrayList<String>();
stringList.add("aa");
stringList.add("bb");
stringList.add("cc");
System.out.println(Joiner.on(",").skipNulls().join(stringList));
```
输出结果
```java
aa,bb,cc
```
除了最基本的连接，当元素为 null 时可以设置忽略或者是用另外的字符串替代。还可以将 map 中的键和值添加分隔符加入到字符串中。
#### Splitter 类
相当于 Joiner 的逆运算。可以在分割时去除空格，可以在分割字符串时指定键和值。

#### 参考书籍
`<<Getting Started with Google Guava>>`