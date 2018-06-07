---
layout:     post
title:      "Two-Pointers"
subtitle:   " \"双指针\""
date:       2018-06-07 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - LeetCode
---

> 双指针问题

#### 141. Linked List Cycle

给定一个链表，判断链表中是否有圈，解决思路很简单，但是很难想到，使用快慢指针，假定快指针每次移动两个结点的距离，慢指针每次只移动一个结点的距离，保持这样的速度移动下去，将来的某个时间，快指针和慢指针会发生套圈现象，快指针和慢指针会相遇。

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while(fast != null && fast.next != null){ // 只要发现 fast 为 null 或者是 fast.next 为 null， 说明 fast 指针 和 slow 指针没有相遇之前， fast 指针已经到达链表终点，链表没有圈
            slow = slow.next;
            fast = fast.next.next;
            if(slow == fast)
                return true;
        }
        return false;
    }
}
```

#### 142. Linked List Cycle II

这道题目是 141 题的升级版，不仅需要判断是否有圈，还需要找到圈开始的位置。

我们还是先使用 141 题目的代码先判断是圈，然后根据快指针、慢指针和头结点的相对位置，找到圈开始的位置。

首先设链表长度为 L，当 fast = slow 时，设 slow 移动的距离为 x，则根据速度可以推测，fast 的移动距离为 2x，可以得知 x 是圈的周长，另外，设从头结点到圈开始的距离为 ls，从圈开始位置距离 slow 位置的距离为 lt，明显有以下关系：

首先设链表的长度为 L，设头结点为 H， slow 结点位置为 S，设圈开始位置为 A，设 slow 指针移动的距离为 x，则根据速度可以推测出 fast 的移动距离为 2x，进而可以推测出圈的长度为 x。明显具有以下结论
```java
HA + AS = HS
SA = x - AS
x = HS
```
可以得出
```java
HA = SA
```

现在的 slow 结点再向前移动 SA 的距离就可以到达圈开始的位置，从头结点移动 HA 也就是 SA 的距离，也可以到达圈开始的位置，也就是当 slow = fast 时，第三个指针从头结点出发，和 slow 结点同时前进，最终两个指针将相遇在圈开始的位置 S。

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {
        // 当发现了存在圈的时候，如何找到圈的起点？
        // 追上时，就是套圈的时候，两个指针的差 diff 就是圈的倍数，因此，得到了圈的倍数之后，从头开始遍历
        // 指针 p 1固定，指针 p2 向前推进 diff，如果相同，说明，该点是起点
        // 使用快慢指针， l1 是快指针， l2 是慢指针
        // 存在等量关系
        // https://blog.csdn.net/liuchonge/article/details/74643929
        
        int i = 0;
        int j = 0;
        
        ListNode slow = head;
        ListNode fast = head;
        
        while(fast != null && fast.next != null){
             slow = slow.next;
             i++;
             fast = fast.next.next;
             j += 2;
             if(slow == fast)
             {
                 return find(head,slow);
             }
        }
        return null;
    }
    
     public ListNode find(ListNode head, ListNode slow) {
        ListNode newNode = head;
        while(true){
            if(newNode == slow)
                return newNode;
            newNode = newNode.next;
            slow = slow.next;
        }
     }
}
```

