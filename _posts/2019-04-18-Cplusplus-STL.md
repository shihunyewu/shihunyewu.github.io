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
### 序列容器
#### vector
#### list
#### deque
#### heap
#### queue
#### priority_queue
##### 基本使用
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
