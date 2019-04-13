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
ArrayList 底层是使用数组实现的。这里 transient 关键字修饰 elementData 数组，主要是因为 elementData 并非总是满的，每次序列化时不应该直接将该数组序列化，因此加了 transient 关键字。
```java
transient Object[] elementData; // non-private to simplify nested class access
```
初始大小为 10。
```java
private static final int DEFAULT_CAPACITY = 10;
```
当需要扩容时
```java
private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
```
可以看出 ArrayList 每次扩容 ` int newCapacity = oldCapacity + (oldCapacity >> 1);`，扩容 1.5 倍。然后将原来维护的数组拷贝到新创建的空间中`elementData = Arrays.copyOf(elementData, newCapacity);`。因此如果提前知道 ArrayList 的大小，可以在初始化 ArrayList 的时候指定。
#### 删除元素
删除指定位置元素后，会将该位置之后的元素都前进一个位置。
```java
 private void fastRemove(int index) {
        modCount++;
        int numMoved = size - index - 1;
        // 不是最后一个元素的时候，将被删除元素之后的元素挨个前移
        if (numMoved > 0)
            System.arraycopy(elementData, index+1, elementData, index,
                             numMoved);
        elementData[--size] = null; // clear to let GC do its work
    }
```
#### fast-fail 机制
ArrayList 类从  AbstractList 类中继承而来 `protected transient int modCount = 0;`，每次改变 ArrayList 数组中的元素时都递增 `modCount` 的值。主要是在 `writeObject` 函数中检查序列化前后 ArrayList 是否发生了变化。
```java
private void writeObject(java.io.ObjectOutputStream s)
        throws java.io.IOException{
        // 缓存序列化之前的 modCount 值
        int expectedModCount = modCount;
        s.defaultWriteObject();

        // Write out size as capacity for behavioural compatibility with clone()
        s.writeInt(size);

        // Write out all elements in the proper order.
        for (int i=0; i<size; i++) {
            s.writeObject(elementData[i]);
        }
		// 序列化之后，查看 modCount 的值是否和序列化之前的相同
        // 如果不相同，说明在序列化期间，有别的线程对 ArrayList 进行了修改
        // 因此抛出异常
        if (modCount != expectedModCount) {
            throw new ConcurrentModificationException();
        }
    }
```

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






