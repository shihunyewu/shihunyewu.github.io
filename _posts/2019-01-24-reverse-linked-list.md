---
layout:     post
title:      "reverse-part-linked-list"
subtitle:   " \"翻转部分链表\""
date:       2019-01-19 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - LEETCODE
---
> 链表就地转置之定点部分转置
### 92. Reverse Linked List II
边界值值得思考

大神版本
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode reverseBetween(ListNode head, int m, int n) {
        if (head == null) return null;
        ListNode newHead = new ListNode(0);
        newHead.next = head;
        ListNode pre = newHead;
        
        for (int i = 0; i < m-1; i++) pre = pre.next;
        
        ListNode start = pre.next;
        ListNode after = start.next;
        
        for (int i = 0; i < n-m; i++) {
            start.next = after.next;
            after.next = pre.next;
            pre.next = after;
            after = start.next;
        }
        
        return newHead.next;
    }
}
```
我的版本
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode reverseBetween(ListNode head, int m, int n) {
        ListNode p0 = head;
        ListNode pre = new ListNode(0);
        pre.next = head;
        ListNode p1 = head.next;
        if(p1 == null)
            return head;
        
        int i = 0;
        for(i = 0; i < m - 1; i++){
            pre = pre.next;
            p0 = p0.next;
        }
        
        ListNode tmpHead = p0;
        p1 = p0.next;
        
        while(i++ < n - 1){
            tmpHead.next = p1.next;
            p1.next = p0;
            p0 = p1;
            p1 = tmpHead.next;
        }
        
        if(pre.next != head){
            pre.next = p0;
            return head;
        }else
            return p0;
        
    }
}
```
### [25. Reverse Nodes in k-Group 分组转置](https://leetcode.com/problems/reverse-nodes-in-k-group/)
迭代调用上面的操作即可，将每一段转置之后连接到新的链表上，这是主要思路。这个题目测试用例的一个细节的就是如果是剩余的节点数不足 k，不转置，这一点需要注意一下。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    ListNode h = new ListNode(0);
    ListNode c;
    int count = 0;
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode p = head;
        while(p != null){
            count++;
            p = p.next;
        }
        ListNode ret = sub(head, k);
        ListNode pre = ret;
        while(this.c != null){  // 终止条件判断
            while(pre.next != null){
                // System.out.println(pre.val);
                pre = pre.next;
            }
            ListNode tmp = sub(this.c, k);
            pre.next = tmp;
            pre = tmp;
        }
        return ret;
    }

    ListNode sub(ListNode head, int k){
        h.next = null;
        ListNode c = head;
        ListNode n = head;

        if(count - k < 0){
            this.c = null;   // 至关重要的终止条件
            return head;
        }
        else
            count -= k;

        for(int i = 0; i < k; i++){
            if(n == null){
                break;
            }
            n = n.next;
            c.next = h.next;
            h.next = c;
            c = n;
        }
        this.c = c; // 至关重要的终止条件
        return h.next;
    }
}
```