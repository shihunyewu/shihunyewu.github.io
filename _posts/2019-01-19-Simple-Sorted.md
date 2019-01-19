---
layout:     post
title:      "Simple-Sorted"
subtitle:   " \"简单排序问题\""
date:       2018-01-19 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - LeetCode
---
> 情况简单的排序方法

[75. Sort Colors](https://leetcode.com/problems/sort-colors/description/)

总结一下题意就是说数组中只有出现 0,1,2 三个数字，对这个数组排序，禁止使用 api 排序，禁止统计 0 和 1 出现的次数，后按照出现次数依次重新赋值。要求遍历一遍就完成排序。

#### 第一种方法：使用三个指针
- l 指针代表从左到右第一个非 0 的元素的位置
- h 指针代表从右到左第一个非 2 的元素的位置
- i 表示当前遍历到的位置
	- 遇到 0 ，就和 l 位置的非 0 元素（实际上只可能是 1，如果是 2 的话就被交换到后面去了）交换，考虑到非 0 元素一定是 1，i 和 l 共同自增；
	- 遇到 1，i 自增；
	- 遇到 2，就和 h 位置的非 2 元素交换，此处的非 2 元素可能是 0 也可能是 1，因此这里交换后，i 不自增

```java
class Solution {
    public void sortColors(int[] nums) {
        int l = 0;
        int h = nums.length - 1;
        // 存放两个指针, l 指针和 h 指针，l 表示第一个非 0 的元素， h 表示第一个为 2 的元素
        int i = 0;
        while(i <= h){
            if(nums[i] == 0){
                swap(nums, i, l);
                i++;
                l++;
            }else if(nums[i] == 1){
                i++;
            }else{
                swap(nums, i, h);
                h--;
            }
        }
    }

    private void swap(int[] nums, int i, int j){
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
}
```


#### 第二种解法：平移
移动过程中一直保持着一个长度为 i 的排好序的 {0,1,2} 数组，每次平移一个元素都会将其添加到对应位置，后面的元素后移。
此处的后移，对于 1,1,1,1 这种序列来说，相当于在将第一个 1 修改成 0，然后在列表最后添加一个 1。总之理解了这种方式的后移就会理解下面代码的含义了。

```java
class Solution {
    public void sortColors(int[] nums) {
        int i = -1;
        int j = -1;
        int k = -1;
        int n = nums.length;
        for(int p = 0; p < n; p ++)
        {
            if(nums[p] == 0)
            {
                nums[++k] = 2;    //2往后挪
                nums[++j] = 1;    //1往后挪
                nums[++i] = 0;    //0 往后加一个
            }
            else if(nums[p] == 1)
            {
                nums[++k] = 2;    // 2 往后挪
                nums[++j] = 1;    // 1 往后加一个
            }
            else
                nums[++k] = 2;    // 2 往后加一个
        }
    }
}
```

#### 扩展
假如数组中只有 4 种数字或者 n 种数字，也可以用平移来求解，当然如果 n 太大，不如直接排序。


[参考](https://www.cnblogs.com/ganganloveu/p/3703746.html)
