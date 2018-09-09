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

[参考](https://blog.csdn.net/xzp_12345/article/details/79251174)

* ArrayList，可变长列表
	* 非同步实现，线程不安全
	* 使用数组实现
	* 每次数组扩容都需要将老数组复制到新数组，每次数组扩容后，新数组约为老数组的1.5倍
	* 使用 Fail-Fast 机制，面对并发的修改，很快表示这样不行
	* remove 只是将 size 减小 1，并将被删除元素位置置为 null

* LinkedList
	* 描述：
		* 继承于 AbstractSequentialList 的双向链表。
		* 实现了 List 接口，能做队列用
		* 实现了 Deque 接口，能做双端队列
		* 实现了 Cloneable接口，能克隆
		* 实现了 Serialiable 接口，可以做序列化
		* 非同步
	* 使用 Node<E> 结点实现，Node 中包含成员变量 item, prev, next
	* 查找，如果 index < size/2， 从头开始找，否则从而指针开始找。

* HashMap
	* 实现

	HashMap 是基于哈希表的 Map接口的非同步实现，允许 null 值映射 null，但是不能保证映射的顺序。
    * 结构
    	* 数组散列结构，总体是数组，单个数组元素是链表
    	* 当链表长度超过阈值 8 时，将链表转换为 红黑树，减少查找时间。
    * 扩容时需要重新计算扩容后每个元素存储的位置，相当于重新创建了一遍
    * 采用 Fail-fast 机制，关键就是 modCount，迭代器初始化过程中，会赋值给迭代器的 expectedModeCount，在迭代过程中，判断 modCount 和 exceptModeCount 是否相等，不相等就是有其他线程修改了 Map
    	* ArrayList 应该也是使用了相同的策略

* Hashtable
	* 同步，并发时，锁住整张 Hash 表
	* 数组，单链表，没有红黑树

* HashSet
	* 使用 HashMap 实现，其中 value 值用 Object对象 PERSNT 代替。

* ConcurrentHashMap
	* 特点：
		* 使用多个锁控制 hash 表的不同段的修改，实际上就是将 hash表分成了多个子hash表。
	* 这个没用过，所以就懒得看。

* Vector
	1. Vector实际上是通过一个数组去保存数据的。当我们构造Vecotr时；若使用默认构造函数，则Vector的默认容量大小是10。

    2. 当Vector容量不足以容纳全部元素时，Vector的容量会增加。若容量增加系数 大于0，则将容量的值增加“容量增加系数”；否则，将容量大小增加一倍。

    3. Vector的克隆函数，即是将全部元素克隆到一个数组中。

    4. 很多方法都加入了synchronized同步语句，来保证线程安全。

    5. 同样在查找给定元素索引值等的方法中，源码都将该元素的值分为null和不为null两种情况处理，Vector中也允许元素为null。

    6. 遍历Vector，使用索引的随机访问方式最快，使用迭代器最慢。

    7. Vector很多地方都与ArrayList实现大同小异，现在已经基本不再使用。

* Stack
	* api只有四个方法
		```java
		boolean empty()
      测试堆栈是否为空。
        E   peek()
              查看堆栈顶部的对象，但不从堆栈中移除它。
        E   pop()
              移除堆栈顶部的对象，并作为此函数的值返回该对象。
        E   push(E item)
              把项压入堆栈顶部。
        int search(Object o)
              返回对象在堆栈中的位置，以 1 为基数。
      ```
	* 描述
		* 继承 Vecotor 实现的，可以使用 Vector 的属性和方法
		* 线程安全，是同步的

* TreeMap
	* 红黑树实现，key值 自动有序
	* 查询，插入，删除均比 HashMap 效率低，最大优点是有序
	* key 值不能为 null，HashMap 的 key 可以为 null
	* 不同步
		* 如果要同步，需要在外界加同步
		```java
        SortedMap m = Collections.synchronizedSortedMap(new TreeMap(...));
        ```
	* [参考](https://blog.csdn.net/u010648555/article/details/60476232)

* TreeSet
	* 使用 TreeMap 实现。
	* [参考](https://blog.csdn.net/itmyhome1990/article/details/76549163)

