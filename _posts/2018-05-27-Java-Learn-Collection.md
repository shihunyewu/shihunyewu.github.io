---
layout:     post
title:      "Java-Learn"
subtitle:   " \"Java 学习中 —— 集合\""
date:       2018-05-27 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - Java
---

## 9 章 集合

##### 9.2 具体的集合

| 集合类型 | 描述 |
|--------|--------|
|    ArrayList    |   一种可以动态增长和缩减的索引序列     |
|	LinkedList	  | 	一种可以在任何位置进行高效地插入和删除操作的有序序列 |
|   Hashtable	  |  哈希表，同时保存键和值 |
|	ArrayDeque	  | 双端队列，可以从两端入队出队 |
|	Stack		| 栈 	|

* 链表

LinkedList 类，然后借助　Iterator 接口来实现元素的遍历
```java
List<String> staff = new LinkedList<String>();
staff.add("I am the first");
staff.add("I am the second");
// 返回 ListIterator，作为一个指针，来辅助访问 List 中的元素
ListIterator<String> iter = staff.iterator();
iter.add("zero");
// 已经将元素添加到了列表头部
String first = iter.next();
System.out.println(first);
// 现在迭代器指向 first，remove的时候，删除的也是 first
//		iter.remove();
String second = iter.next();
System.out.println(second);
// LinkedList 是一个双向链表，可以指向前一个元素
String pre = iter.previous();
System.out.println(pre);
for (String string : staff) {
	System.out.println(string);
}
String oldValue = iter.next();
iter.set(newValue); // 将 iter 指向的元素替换成一个新的值
// 使用 contain 方法判断是否存在某个元素
System.out.println(staff.contains("zero"));
// 使用 get(int i) 方法访问第 i 个元素，List 未做缓存，无优化，因此复杂度为 O(n)，应该谨慎使用
// 而使用 iter.next() 和 iter.previous() 方法效率很高，应为 iter 一直保持当前的计数
System.out.println(staff.get(0));
// 如果有一个整数索引 n，使用 listIterator(n).next()，会返回索引为 n 元素，同时指针下移
System.out.println(staff.listIterator(1).next());
// LinkedList 类的方法还有 addFirst(Object),addLast(Object)
// getFirst(),getLast(),removeFirst(),removeLast() 等方法
LinkedList<String> staffL = (LinkedList<String>)staff;
staffL.addFirst("-first");
staffL.addLast("I am three");
System.out.println(staffL.toString());
```

* 数组列表类

ArrayList 类实现了 List 接口，封装了一个动态再分配的对象数组。Vector 是与 ArrayList 类似的数据结构，并且实现了同步访问，但是同步需要消耗时间和资源，因此在不需要同步的时候，可以选择使用 ArrayList

* 散列集
	* 实现原理：
		* 计算对象的散列码
		* 散列码对桶的总数取余，根据余数放到对应的桶中
		* 如果桶中已有对象，比较对象是否相同，如果不相同，添加，如果相同，不添加
	* 再散列
		* 装填因子为 0.75 ：散列表中超过 75% 的位置已经填入了元素，表就会用双倍的桶数自动进行再散列

* set 类
	* set 类中是没有重复元素的元素集合

* HashSet 类
	* add 添加新元素
	* contains 快速在某个桶中查看元素是否已经存在

* 树集（TreeSet类）
	* 和 HashSet 的区别，TreeSet 顺序保存元素
	* 可以通过实现 Comparable 接口来控制顺序

* 队列和双端队列

* 优先队列

* 映射表
	```java
	Map<String, Employee> staff = new HashMap<String, Employee>();
	```

* 栈(Stack)
	* boolean empty()，测试栈是否为空
	* Object peek(), 查看栈顶的元素，但是不删除
	* Object pop(), 删除栈顶元素，并且删除
	* Object push(Object), 将元素压入栈中
	* int search(Object), 返回元素的位置

