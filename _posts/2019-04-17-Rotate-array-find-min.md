---
layout:     post
title:      "Totete-array-find-min"
subtitle:   " \"旋转数组中查找最小的元素\""
date:       2019-04-17 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - LEETCODE
---

> 现在的不如意都有个不努力的曾经

## [153. Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

旋转数组即将递增数组拦腰折断，将前半部分移到后半部分之后。寻找最小元素，也就是寻找折点。
这道题目改变了我对二分查找的认识，以前对二分查找死记硬背，认为返回值一定是 nums[m]，每次判断之后不是 l = m + 1 就是 h = m - 1。
该题目中的每次位移变成了 l = m 和 h = m，这样比就避免了如果 m 是最小值，l = m+1 越过 m 的问题， h = m - 1 也是同理。
与此同时返回值也变成了当 h - l <= 1 时，返回 Math.min(nums[l], nums[h])，解决了如何判断最小值的问题，我之前的思路是判断 nums[m] 是否同时小于 nums[m - 1] 和 nums[m + 1]，这样非常繁琐，还要考虑边界值，需要考虑 m - 1 < 0 和  m + 1 > nums.length - 1 的情况，而当 l 和 h 相邻或者是 h = l 时，直接返回 Math.min(nums[l], nums[h]) 就非常简洁而且准确。

```java
class Solution {
    public int findMin(int[] nums) {
        /*
        先上升后下降
        */
        if(nums.length == 0)
            return 0;
        if(nums.length == 1)
            return nums[0];
        if(nums.length == 2)
            return Math.min(nums[0], nums[1]);
        int l = 0;
        int h = nums.length - 1;
        int m = (l + h)/2;
        while(true){
            if(h - l <= 1)
                return Math.min(nums[l], nums[h]);
            m = (l + h)/2;
            if(nums[m] > nums[h])
                l = m;
            else
                h = m;
        }
    }
}
```

## [154. Find Minimum in Rotated Sorted Array II](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/)
进阶：可以有重复元素，需要多判断一种情况，nums[m] = nums[h]

```java
class Solution {
    public int findMin(int[] nums) {
        int[] array = nums;
        if(array.length == 0)
            return 0;
        int l = 0;
        int h = array.length - 1;
        while(true){
            if(h - l <=1){
                return Math.min(nums[l], nums[h]);
            }
            int m = (l + h)/2;
            if(nums[m] <= nums[h]){
                if(nums[m] == nums[l])
                    return min(array, l, h);
                h = m;
            }
            else if(nums[m] > nums[h]){
                l = m;
            }
        }
    }
    private int min(int[] array, int s, int e){
        int ret = Integer.MAX_VALUE;
        for(int i = s; i <= e; i++){
            ret = Math.min(array[i], ret);
        }
        return ret;
    }
}
```
