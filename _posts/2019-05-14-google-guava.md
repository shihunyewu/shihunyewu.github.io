---
layout:     post
title:      "google guava"
subtitle:   "Java编程的助手 - guava"
date:       2019-05-15 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - Java
---
> 使用像 Guava 这样的库远远好于自己写

Guava 是 google 开发的一个 java 集合库，这些类库包含字符串、集合、并发、I/O 和 反射工具。Guava 封装了很多样板代码，让开发者的开发更便捷也更不容易在某些细节上出错。
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

#### Charsets 类
里面封装了基本的字符集名，开发者不必再为记不清字符集名发愁了。


#### Strings 类
- Strings.padEnd(String origin, int n, char c)，向 origin 后面添加 n 个字符 c
- Strings.padStart(String origin, int n, char c)，向 origin 前面添加 n 个字符 c
- Strings.nullToEmpty(String object)，将 null 转换成 ""
- Strings.emptyToNull(String object)，将 "" 转换成 null
- Strings.isNullOrEmpty(String object),判断 object 是否是 null 或者是 ""

### 第 4 章，集合类
#### Lists
- Lists.newArrayList(object1, object2, object3,...); 将元素添加到新列表，返回列表
- `List<List<T>> Lists.partition(list, k)`，将 list 等分成 k 份

#### Sets
java 原生 Set 不支持例如求并集、交集这样的集合操作，Sets 提供了相关的 api
- Sets.difference(set1, set2)，求 set1 相对于 set2 的差集
- Sets.symmetricDifference(set1, set2)，求两个集合相互差集的并集
- Sets.intersection(set1, set2)，求两个集合的交集
- Sets.union(s1, s2)，求两个集合的并集

#### Multimaps
允许相同键值存在多个 val
##### ArrayListMultimap
- 用 ArrayList 来保存 val 的 map
- ArrayList 不强制要求 val 都是唯一的，可以重复
- ArrayListMultimap 没有实现 Map 接口，如果想要将其变成 Map 类型，需要调用其 asMap() 方法

##### HashMultimap
- HashMultimap 基于哈希表，不支持相同的 (key,val)

#### BiMap
- 保持 val 是唯一的，使用 put 方法如果插入了相同的 val 时，会抛出异常
	- 如果一定要插入的话，需要使用 forcePut 方法，将会删除原来的 key，替换成新的 key
- biMap.inverse()，会将 biMap 中的 key 和 val 反转，key 变成  val，val 变成  key

#### Table
保存了 map 的 map，底层数据结构是 `Map<R, Map<K, V>>`
- HashBasedTable<Integer,Integer,String> table = HashBasedTable.create(5,5);


#### Range
用来比较是否在某一区间，可以选择区间两端的开闭
- open，全开区间
- close，全闭区间
- openClosed，半开半闭
- closeOpen，半闭半开
- ...
对任意类型都可以使用区间，只要该类实现了 Comparable 接口。实际上现在 java8 的流式编程已经能够很好地处理这种情况了。

#### 参考书籍
`<<Getting Started with Google Guava>>`