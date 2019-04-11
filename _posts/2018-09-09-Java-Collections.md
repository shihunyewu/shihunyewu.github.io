---
layout:     post
title:      "Java-Collections"
subtitle:   " \"Java 集合类的基本使用和内部实现\""
date:       2018-09-09 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - Java
---

> 每次看都应该有不同的体会

## 概览
总体上来看集合应该分为两种 Collection 和 Map
### Collection
- 列表
	- ArrayList，可变长数组
	- LinkedList，基于双向链表实现
	- Vector，类似于 ArrayList，源码中使用了 synchronize，线程安全
- 队列
	- Queue，使用 LinkedList 实现了 Queue 接口
	- PriorityQueue，优先队列，使用堆来实现
- 集合
	- HashSet，基于 HashMap 实现，查找复杂度为 O(n)
	- TreeSet，基于 TreeMap 实现，键值有序，查找时间复杂度 O(log2(n))

### Map
- HashMap，用拉链法解决哈希冲突，非线程安全
- TreeMap，用红黑树实现，非线程安全
- HashTable，使用了 synchronize 关键字，线程安全，效率低，每次访问锁定整个表
- ConcurrentHashMap，使用分段锁，线程安全，相对于 HashTable，效率高
- LinkedHashMap，使用双向链表维护键的顺序

## 具体
### ArrayList
#### 常用成员函数
1. add(T t)
2. remover(T t)/remover(int index)

#### 如何扩容
#### 删除元素
#### fast-fail 机制

### LinkedList
#### 常用函数
1. add(T t)
2. remover(T t)/remover(int index)

### Vector
####  常用函数
1. add(T t)
2. remover(T t)/remover(int index)


### Queue
#### 常用函数
1. add(T t)/offer(T t)，添加一个元素，其实 offer 函数内部也是用 add 实现的
2. poll()，返回第一个元素并删除，如果没有，返回 null
3. peek()，返回第一个元素，如果没有，返回 null

### PriorityQueue
#### 常用函数
1. add(T t)/offer(T t)，添加一个元素，其实 offer 函数内部也是用 add 实现的
2. poll()，返回第一个元素并删除，如果没有，返回 null
3. peek()，返回第一个元素，如果没有，返回 null


#### 如何实现有序

### HashSet
#### 常用函数
1. add(T t)
2. contains(T t)


#### 如何计算 hashCode

### TreeSet
#### 常用函数
1. add(T t)
2. contains(T t)

### HashMap
#### 常用成员函数
1. put(key, val)
2. get(key)
3. containsKey(key)


#### 如何扩容

### TreeMap
#### 常用成员函数
1. put(key, val)
2. get(key)
3. containsKey(key)

#### key 如何有序


### ConcurrentHashMap
#### 常用成员函数
1. put(key, val)
2. get(key)
3. containsKey(key)

#### 分段锁如何实现

### LinkedHashMap
#### 常用成员函数
1. put(key, val)
2. get(key)
3. containsKey(key)

#### 如何实现 LRU






