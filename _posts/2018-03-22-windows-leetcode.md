---
layout:     post
title:      "windows-leetcode"
subtitle:   " \"windows\""
date:       2018-03-22 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - LEETCODE
---

> "滑动窗口总结"

刷LEETCODE的时候按照相关性刷题，遇到某一方面问题之后，从易到难，这样会加深自己该方面的认识，另外思路也比较好找。这次遇到的是滑动窗口这类的问题。

#### 209. Minimum Size Subarray Sum
[题目地址](https://leetcode.com/problems/minimum-size-subarray-sum/description/)

该题目要求的是，找到和超过所给整数s的最小长度连续数组。
题目的思路就是，从数组最左边元素向右依次求和，直到大于s之后，左指针开始向右缩进，更新到子数组的和不再大于 s 为止，之后继续向右遍历。

相应代码：
```python
class Solution(object):
    def minSubArrayLen(self, s, nums):
        if sum(nums) < s:
            return 0
        col = 0
        minn = len(nums)
        rn = 0
        k = 0
        for i in range(len(nums)):
            col += nums[i]
            rn += 1
            while col >= s:
                if minn > rn:
                    minn = rn
                col -= nums[k]
                rn -= 1
                k += 1
        return minn
```

#### 76. Minimum Window Substring
[题目地址](https://leetcode.com/problems/minimum-window-substring/description/)
该题目是在字符串 s 上寻找字符串 t 的长度最小的覆盖子串。
思路和上一题目的思路相似，
我一开始卡在了
```python
s = "a"
t = "aa"
```
这个测试用例上，当时是因为我只是使用hash表存储了 t 中每个字母是否出现，然后使用哈希表中是否存在 0 值来判断 t 中的所有字母是否都已经出现过一次，因为t中可能存在相同的字符，所以只记录是否出现时不够的。所以后来改成使用hash表来存储 t 中每个字母出现的次数，使用一个初始值为 len(t) 的标志是否为 0 值来判断 t 中的所有字母是否都已经出现过一次。后来就AC了。
感悟就是有的时候多开辟一个变量的空间会节省很多事情。
详细代码和详细注释如下：

```python
class Solution(object):
    def minWindow(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: str
        """
        # 如果  s 为空值，直接返回 ""
        if len(s) == 0:
            return ""
        # 如果 t 为空值，直接返回""
        if len(t) == 0:
            return s[0]
        # 包含字符串最左边的临时索引
        left = 0
        # 最小包含字符串的最左边索引
        rl = 0
        # 临时存储长度
        rn = 0
        # 设置最小字符串长度为 len(s) + 1
        minn = len(s)+1
        # num 作为 是否已经找到t中全部字符串的标志
        num = len(t)
        # 字典d，用来保存t中每个字母在 t中出现的次数
        d = {}
        for i in t:
            d[i] = 0
        for i in t:
            d[i] += 1
        for i in range(len(s)):
            if s[i] in t:
                if d[s[i]]>0:
                    # 如果 s[i] 存在于 t中，并且 d[s[i]] > 0，这说明  这个字母还没有被遍历够
                    # 遍历次数达到 s[i] 在t中出现的次数时， d[s[i]] = 0
                    # 小于 s[i] 的时候，d[s[i]] < 0
                    # >0 时，是有效遍历，所以总次数减 1
                    num -= 1
                # 将s[i] 这个字母的哈希值 减 1，表示又碰见了一次
                d[s[i]] -= 1
            # 包含字符串长度加 1
            rn += 1
            # 当 t 中所有的字符串都在 s中找到了之后，开始将左索引向右移动
            while num == 0:
                # 判断 minn 和rn之间的大小
                if minn > rn:
                    # 更新最左索引
                    rl = left
                    # 更新最小字符串长度
                    minn = rn
                # 如果 s[left] 在 t中出现
                if s[left] in t:
                    # 将s[left] 的哈希值 加 1
                    d[s[left]] += 1
                    # 如果 s[left] 的哈希值已经 等于 0了，处于一个临界值，进入下次循环
                    if d[s[left]] < 1:
                        left += 1
                        rn -= 1
                        continue
                    else:
                        # 如果 s[left] 的哈希值已经 大于 0 了，那么现在的字符串已经不符合条件了，跳出循环
                        left += 1
                        rn -= 1
                        num += 1
                        break
                else:
                    # 如果不存在于 t中，那么左索引直接右移，最小字符串长度减 1
                    left += 1
                    rn -= 1
                    continue
        # 如果 minn值 从未被更新过，说明 t 不被包含于 s中
        if minn == len(s)+1:
            return ""
        else:
            # 否则返回
            return s[rl:rl+minn]
```