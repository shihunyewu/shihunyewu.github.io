---
layout:     post
title:      "Count Binary Substrings"
subtitle:   " \"二进制字符串计数\""
date:       2018-04-11 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - LEETCODE
---

> "这道题目不难，看到了很简洁的解法"

#### 696. Count Binary Substrings
[题目地址](https://leetcode.com/problems/count-binary-substrings/description/)

主要就是求字符串 s 中，有多少个 '01','10','..0011..','..1100..'，这样的字符串。

比较合乎思维定式的思路就是每次碰到 s 中的字符 '1' 就开始前后延伸.

> 其他代码更精简的实现版本不过是这种思路的一种异化。上面这种实现方式，可读性更强。
