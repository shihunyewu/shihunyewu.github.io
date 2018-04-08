---
layout:     post
title:      "Find No-repeated Elements"
subtitle:   " \"寻找非重复元素\""
date:       2018-04-05 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - LEETCODE
---

> "寻找非重复元素"

随便Pick One，就 Pick 到了Single Number III。所以顺便将 Single Number I 和 Single Number II 也一起做了个总结。

#### 136. Single Number
[题目地址](https://leetcode.com/problems/single-number/description/)

这道题目是一道热身题，题目中说给定一个数组，其中除一个元素只出现过一次外，其他元素都出现过两次，求这个只出现过一次的元素。
该题目的关键思路就是使用异或，本题目中主要使用的异或操作的两个特点是
```python
111 ^ 111 = 0 # 可以将出现两次的元素异或成 0
0 ^ 111 = 111 # 数字和 0 异或，结果还是原来数字
```

相应代码：
```python
class Solution(object):
    def singleNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        tmp = 0
        for num in nums:
            tmp ^= num
        return tmp
```

#### 260. Single Number III
[题目地址](https://leetcode.com/problems/single-number-iii/description/)

这个题目是 Single Number I 的进阶，该题目中，只出现一次的数字变成了两个，要求的也就是这两个数字。
如果使用 Single Number I 中的方法，最后得到的 result_num 是待求数字 a 和 待求数字 b，这两个数字的异或结果。
这个 result_num 中为 0 的二进制位是 a 和 b 相同的部分，为 1 的二进制位是 a 和 b 不同的部分。
考虑使用 a 和 b 不同的部分来将整个数组划分为两个子数组，然后每一类中再使用 Single Number I 中的求解方法，就可以分别求出 a 和 b。
其中划分标准只需要使用 result_num 中一个为 1 的二进制位即可。
这其实也是分治思想的体现。

相应代码：
```python
class Solution(object):
    def singleNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
		# 日常处理异常数据
        if len(nums) == 0:
            return [0,0]
		
		# 求 a 和 b 的异或结果
        tmp = nums[0]
        for i in xrange(1,len(nums)):
            tmp ^= nums[i]
		
		# 找到一位可以区分 a 和 b 的二进制位
        flag = 1
        while flag&tmp == 0:
            flag = flag<<1
		
		# 找 a 和 b
        num_1 = 0
        num_2 = 0
        for i in xrange(0,len(nums)):
            if nums[i] & flag == 0:
                num_1 ^= nums[i]
            else:
                num_2 ^= nums[i]
		
        return [num_1,num_2]
```

#### 137. Single Number II
[题目地址](https://leetcode.com/problems/single-number-iii/description/)

这道题目是将数组中数字的重复次数改成了三次。
在网上看到其他人的思路是对数组中数字的相同一个二进制位求和之后对 3 取余，得到的余数就是只重复一次的数字的二进制位的结果。
从这个思路看，其实 Single Number I 中的异或操作其实是对 2 取余。
从这个思路往外扩展，我们可以求数组中数字的重复次数为 4次，5次，，n次。
另外对于题目中只出现过一次的数字，我们也可以改成出现为 m(m < n) 次。
这类问题都可以用这种思路来求解。

相应代码：
```python
class Solution(object):
    def singleNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        res = 0
        for i in range(32): # 遍历每个数字的 32 位二进制位
            cnt = 0
            mask = 1 << i
            for num in nums: # 累加数组中每个数字的第 i 位二进制位
                if num & mask:
                    cnt += 1
            res |= (cnt % 3) # 将相应二进制位上的结果通过 或操作 赋值到 res 上
        if res >= 2 ** 31: # 将无符号整数转换成有符号整数
            res -= 2 ** 32
        return res
```

[代码参照](https://blog.csdn.net/fuxuemingzhu/article/details/79554959)
> 其他代码更精简的实现版本不过是这种思路的一种异化。上面这种实现方式，可读性更强。
