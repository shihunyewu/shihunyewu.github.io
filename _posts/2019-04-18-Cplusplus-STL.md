---
layout:     post
title:      "C++-STL"
subtitle:   " \"C++ STL 的基本使用\""
date:       2019-04-18 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - C++
---
> 解开困扰自己已久的疑惑，更能满足自己的好奇心

## C++ STL
### 概述
C++ STL 的容器分为两类，一类是序列式容器，一类是关联容器。
### 1. 序列容器
#### vector
##### 基本介绍
- 支持随机访问
- 底层一片连续的空间
- 适用于高频访问，低频插入

##### 常用方法
- push_back(elem), 在尾部添加一个元素
- pop_back(), 删除最后一个元素
- front(), 获取第一个元素
- back(), 获取最后一个元素
- begin(),end(), 获取开始迭代器和结束迭代器
- rbegin(),rend(), 获取逆序开始迭代器和逆序结束迭代器
- size(), 返回数组大小
- empty(), 是否为空
- insert(), 插入元素，可以在某一个位置插入单个元素的多个副本，可以接受其他容器的迭代器
- erase(), 删除指定位置的元素，指定范围的元素
- [], 支持随机访问

#### list
##### 基本介绍
- 双向链表实现
	- 每个节点保存有 prev 和 next 两个指针
- 不支持随机访问
- 适用于大规模插入和删除

##### 常用方法

#### deque
##### 基本介绍
- 双向开口的连续线性空间
	- 可以在头尾两端分别做元素的插入和删除操作
- 支持随机访问
	- 复杂度高于 vector
	- 如果需要对 deque 排序，最好是将 deque 复制到 vector 中，排序后再复制回 deque
#### stack
##### 基本介绍
- 先进先出
- 底层用其他容器实现，比如 list，这种模式属于适配器模式
- 不提供随机访问，不提供迭代器

##### 常用方法
- push()
- pop()
- size()
- top()
- empty()


#### queue
##### 基本介绍
- 先进先出
- 底层用其他容器实现，比如 list
- 不提供随机访问

##### 常用方法
- push()，在队尾添加元素
- pop()，队头出元素
- size()
- empty()
- front()
- back()

#### heap
##### 基本介绍
#### priority_queue
##### 基本介绍
- 实现了 queue 的接口语义
- 内部使用 vector 实现
- 调整堆使用 heap 的相关算法
- 可以自定义排序方式

##### 基本使用
下面实例测试了基本类型建立小顶堆，复杂类型（struct 类型）建立大顶堆和小顶堆
```java
#include <queue>
#include <iostream>
/* #include <algorithm> */

using namespace std;

struct Edge{
    int start,end;
    double weight;
    Edge(int start, int end, int weight):start(start),end(end),weight(weight){};
};
typedef struct Edge Edge;

// 使用 greater<Edge> 时，就是小顶堆
bool operator > (Edge a, Edge b){
    return a.weight > b.weight;
}

/**
使用 < 重新定义了比较函数，使用 less<Edge> 时，就是大顶堆
**/
bool operator < (Edge a, Edge b){
    return a.weight < b.weight;
}


int main(){
    priority_queue<Edge, vector<Edge>, less<Edge>> pqueue_Edge; // less 就是大顶堆
    pqueue_Edge.push(Edge(0, 1, 6));
    pqueue_Edge.push(Edge(2, 1, 4));

    cout<< pqueue_Edge.top().weight <<endl;

    int ia[9] = {0, 1,2,3,4,8,9,3,5};
    /* priority_queue<int> ipq(ia, ia + 9);  如果直接定义的话，那么泛型的后面两项会缺省为 vector<int> 和 less<int>
	默认使用比较器 less<int> 的时候是大顶堆
    */
    priority_queue<int, vector<int>,greater<int>> ipq;
    ipq.push(0);
    ipq.push(10);
    ipq.push(11);
    cout<< "size:" << ipq.size() <<endl;

    for(int i = 0; i < 10; ++i){
        ipq.push(ipq.top() + i);
        cout<< ipq.top() << " ";
    }
    cout<<endl;
    while(!ipq.empty()){
        cout<< ipq.top() <<" ";
        ipq.pop();
    }
    cout<<endl;
}
```

### 2. 关联容器
关联容器主要分为 set 和 map 两种
#### set
##### 基本介绍
- 会根据元素的键值自动排序
- 不可以改变 set 值
- 底层使用 RB-tree  实现

##### 常用方法介绍
#### map
##### 基本介绍
- 根据键值排序，不允许键值相同
- 键和值是 pair
- 不可以通过迭代器修改 key，可以修改 value
- 底层通过 RB-tree 实现，几乎所有的 map 操作只是转调用 RB-tree 的操作行为

#### hash_set
##### 基本介绍
- 底层用 hashtable 实现
	- hashtable 使用二次探测法
	- hashtable 使用拉链法解决冲突

#### hash_map
##### 基本介绍
- 底层使用 hashtable 实现