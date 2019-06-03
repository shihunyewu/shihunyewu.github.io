---
layout:     post
title:      "union find"
subtitle:   " 并查集 "
date:       2019-06-03 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - LEETCODE
    - algorithm
---
##  并查集
并查集是一种森林状数据结构，主要用来判断两个节点是否在同一分支上。
#### 底层数据结构实现
其中常见底层数据结构实现有
- Array，索引即为元素，数组索引对应的值表示根元素（或者上一级节点），一般元素数的范围一定且比较小时用 Array
- HashMap，键为元素，值为根元素（或者上一级节点）

核心思想就是需要保存节点和节点的前驱，保证能够通过前驱找到前驱的前驱，迭代找到当前分支的根节点。而根节点是判断两个节点是否在同一分支上的根本依据。

#### 并查集的常规操作有
0. 初始化并查集
	- 将每个元素的前驱设置为自身，表明每个节点暂时都是根节点。
1. 找到节点所在分支的根节点
	- 如果节点的前驱不等于自身，迭代向前查找前驱的前驱
	- 如果节点的前驱等于自身，说明是根节点
2. 查找两个节点是否在同一分支
	- 用操作 1 找到两个节点各自的根节点
	- 比较两个节点的根节点是否相同
		- 相同，在同一分支
		- 不同，不再同一分支
3. 合并两个不同的分支
	- 用操作 1 找到两个分支的跟节点 l, r
	- 令 r 的前驱等于 l，这样 l 和 r 所代表的分支就合并在一起
		- 因为下次 r 分支上的点再找根节点时，就会找到 l，而 l 分支上的点找根节点本来就会找到 l，如此两个根节点相等，说明是同一分支

#### LEERCODE 相关题目
##### [684. Redundant Connection](https://leetcode.com/problems/redundant-connection/)
题目大意说的是给定给一个图，但是其实想要的是一棵树，判断是哪一条边让树出现了圈。题目的输入是很多边，边的两端是顶点。总体思路是每次从输入中得到一条边，然后判断两个端点是否在同一个分支上，如果不在，那么说明没有环，如果在同一个分支上，说明两个端点之间本来就是相互连通的，添加了新的边之后就会形成环。另外，在判断之后还要兼顾对并查集的构建。具体的实现代码如下：
```java
class Solution {
    public int[] findRedundantConnection(int[][] edges) {
        int[] parent = new int[2001]; // 因为本题目中所给的边的数量最多是 1000，所以顶点最多是 2001 个，可以创建一个数组来存储
        int[] ret = new int[2];
        for(int i = 0; i < parent.length; i++)
            parent[i] = i;
        for(int i = 0; i < edges.length; i++){
            int l = findRoot(edges[i][0], parent);
            int r = findRoot(edges[i][1], parent);
            if(l == r) // 查看找到的根节点是否相同，相同即为在同一分支
                return edges[i];
            parent[l] = r; // 将 l 所在的分支添加到 r 上，相当于合并分支
        }
        return ret;
    }

    int findRoot(int p, int[] parent){ // 相当于查找操作
        int k = p;
        while(parent[p] != p)
            p = parent[p];
        parent[k] = p; // 每次查找完之后，顺便将初始查找节点的前驱直接赋值为根节点，减少以后的查找次数
        return p;
    }
}
```


#### 参考
1. [LeetCode并查集(UnionFind)小结](https://www.jianshu.com/p/def7767982ac)
2. [超有爱的并查集~](https://blog.csdn.net/niushuai666/article/details/6662911)