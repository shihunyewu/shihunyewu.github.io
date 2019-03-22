---
layout:     post
title:      "Longest Substring"
subtitle:   " \"最长子串问题\""
date:       2019-01-19 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - LeetCode
---
> 自入题海以来，饱受X*最长的子串问题的困扰

### 3. Longest Substring Without Repeating Characters
该题目求的是最长非重复子串。直接使用暴力方法计算所有的可能子串，时间复杂度是O(n^2)。
想用空间换时间。首先需要记录当前在统计的子串的起始位置 start ，另外要能够判断子串的终止位置，记录最长子串的长度 ret。
现在可以用一个 HashMap 来存储每个字符 c 最近一次遇到时的位置 k。
- 当在 i 位置再次遇到字符 c 的时候，更新 start 为 k + 1，因为这期间从来没有出现重复字符串，只是 k 位置出现了和 i 位置相同的字符，k 位置的字符和 i 位置的字符只能存留一个，为了往下继续寻找潜在的最长子串，只能是保留 i ，然后从 k + 1 开始计算新的子串长度。
- 当没有遇到 c 时，最长不重复子串的长度一直增长。

以下是具体代码。

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int len = s.length();
        if(len == 0)
            return 0;
        Map<Character, Integer> m = new HashMap<Character, Integer>();
        int start = 0;
        int ret = 1;
        for(int i = 0;i < len;i++){
            if(!m.containsKey(s.charAt(i)) || m.get(s.charAt(i)) < start){
                m.put(s.charAt(i), i);
                ret = i - start + 1 > ret ? i - start + 1 : ret;
            }else{
                int tmp = m.get(s.charAt(i));
                start = tmp + 1;
                m.put(s.charAt(i), i);
            }
        }
        return ret;
    }
}
```

### 53 Maximum Subarray
求连续数组的最大和











