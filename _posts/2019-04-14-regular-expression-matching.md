---
layout:     post
title:      "regular-expression-matching"
subtitle:   " \"leetcode \""
date:       2019-04-14 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - LEETCODE
---
> 有的问题是值得花时间去深究的

## 10. Regular Expression Matching

### 递归版本
从题意中得知，一共有三种情况
1. "." 可以匹配任何字符，直接跳过
2. 普通字符，直接匹配
3. "*" 必须要和前面的字符一起考虑，因此当遍历到字符 c 的时候，检查后一个字符是否为 *，如果为 *，那么就存在两种策略，一种是匹配 0 次，一种是匹配一次，递归调用匹配函数

下面是具体代码：
```java
 public boolean isMatch(String s, String p){
        if (p.length() <= 0) return s.length() <= 0; // 如果匹配串的长度为 0，检查被匹配串长度是否 0
        boolean match = (s.length() > 0 && (s.charAt(0) == p.charAt(0) || p.charAt(0) == '.')); // 是否是第 1,2 两种情况
        if (p.length() > 1 && p.charAt(1) == '*'){ // 如果 p 的 长度大于 1 并且 p.charAt(0) 后面的字符是 '*'
        	// 有两种选择
            // 一种是，直接跳过
        	// 一种是匹配一个，在下一次递归操作的时候，就可以遍历匹配 1次，再下次递归将会匹配 2 次，再下次将匹配 n 次
            // 第二种将会匹配到 a* 和 aaaaaaaaaaaaa 这种情况
            return isMatch(s, p.substring(2)) || (match && isMatch(s.substring(1), p));
        } else {
        	// 如果不是，那么继续比较，各自增 1
            return match && isMatch(s.substring(1), p.substring(1));
        }
    }
```

[参考](https://www.cnblogs.com/mfrank/p/10472663.html)