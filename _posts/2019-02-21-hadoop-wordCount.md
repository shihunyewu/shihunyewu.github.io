---
layout:     post
title:      "Simple-Sorted"
subtitle:   " \"简单排序问题\""
date:       2019-01-19 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - LeetCode
---
> 寻找滑动窗口中的最大值

```java
import java.util.LinkedList;
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> maxInWindows(int [] num, int size)
    {
        // 当 i - size < 0 时，直接将其添加双向列表中
        // 队尾添加元素之前，先向前删除节点，直到节点的值大于 i 位置
        // 队头出队的时候，先将位置小于 i - size  的元素都出队，因为该最大值已经在滑动窗口之外
        
        LinkedList<Integer> l = new LinkedList<Integer>();
        ArrayList<Integer> ret = new ArrayList<Integer>();
        if(size == 0)
            return ret;
        for(int i = 0; i < num.length; i++){
            while(!l.isEmpty() && num[l.peekLast()] < num[i]) 
                 l.pollLast();
            l.add(i);
            if(i - size + 1 >= 0){
                while(!l.isEmpty() && l.peekFirst() < i - size + 1 )
                    l.pollFirst();
                ret.add(num[l.peekFirst()]);
            }
        }
        return ret;
    }
}
```