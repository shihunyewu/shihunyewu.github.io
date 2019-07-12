---
layout:     post
title:      "knapsack problem"
subtitle:   "背包问题"
date:       2019-05-19 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - LEETCODE
    - Knapsack
---

> 做题目不是最终目的，通过做题发现知识盲区，去研究学习，才能不断提高。

## 背包问题
### 01背包问题
有 N 件物品和一个容量为 total 的背包。第 i 件物品的质量为是w[i]，价值是v[i]。求解将哪些物品装入背包可使这些物品的费用总和不超过背包容量，且价值总和最大。
```java
public class Knapack {

	public static void main(String[] args) {
		int[]v = {12,3,10,3,6}; // v 是价值
		int[]w = {5,4,7,2,6}; // w 是质量
		int total = 15;
		System.out.println(maxValue(v, w, total));
	}
	/**
	 * 实际上就是一个二维动态规划
	 * @param v，价值
	 * @param w，质量
	 * @param total
	 * @return
	 */
	static public int maxValue(int[] v, int[] w, int total) {
		int N = v.length;
		int[][] dp = new int[N + 1][total + 1];
		// [i, j] 表示 当 容量为 j 时，所给物品为 {1, 2, 3, ... i} 时的最大价值
		for(int i = 1; i < N + 1; i++) {
			for(int j = 1; j < total + 1; j++) {
				if(j < w[i - 1])  // 此时 j 代表着当前总容量，当总容量小于物品 i 的容量时，直接不考虑物品 i
					dp[i][j] = dp[i - 1][j];
				else // 放进第 ith 物品的代价就是减少 w[i - 1]（ith 物品的质量） 后的最大收益  f[i-1][j-w[i]] 加上 v[i -1]（ith 物品的价值）
					dp[i][j] = Math.max(dp[i-1][j], dp[i-1][j - w[i-1]] + v[i-1]);
			}
		}
		return dp[N][total];
	}
}
```

## [1049. Last Stone Weight II](https://leetcode.com/contest/weekly-contest-137/problems/last-stone-weight-ii/)
题目大意说是有一堆石头重量不一，每次取重量分别为 x, y 的两块石头相撞，x < y 时，x 的石头粉碎，y 的石头质量变成 y - x，如果 x = y，两块石头都被粉碎。多次进行上次操作，求取到最后的剩下的石头最轻是多重。

#### 解题思路
参考 2 中的作者言简意赅，作为一个小白还是要思索良久才能搞明白。作者在文中说
> 思路：可以等价为为每个数添加正负号，然后求和，然后求 “和最小的情况”

可以等价为为每个数添加正负号，也就是说假设序列是 `[31,26,33,21,40]`，我们要做的就是将在每个数字前面填上指定的正负号，举个例子就是将原来的序列变成这样 `-31 + 26 + 33 - 21 + 40`，然后求和最小的填法。

> 这不就是背包问题的变形吗

转折及其之快。。让人措不及防。。小白根本跟不上大神的脑回路。。还好大神将代码贴出来了。。认真研究后发现，详细的思路是这样的。
- 将取正号和取负号的数字分为两类，目标就是努力让两个集合的和的差值最小。
	- 计算出总集合的和，取背包容量为总集合和的大小的一半，这样使用 01 背包算法就能够计算出在此容量下的最大的取值 maxValue
		- 可以知道取正数的集合的和为 (sum - maxValue)，取负数的集合的和为 maxValue
		- answer = (sum - maxValue) - maxValue;

下面是 java 版本的代码
```java
class Solution {
    public int lastStoneWeightII(int[] stones) {
        int sum = 0; // 计算总值
        for(int i = 0; i < stones.length; i++)
            sum += stones[i];
        // 背包容量值
        int target = sum / 2;

        int n = stones.length;
        int[][] dp = new int[n + 1][target + 1];
        for(int i = 1; i < n + 1; i++){
            for(int j = 1; j < target + 1; j++){
                if(j < stones[i - 1]) // 当容量小于 j 时，不考虑石头 i
                    dp[i][j] = dp[i - 1][j];
                else
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i - 1][j - stones[i - 1]] + stones[i - 1]);
            }
        }
		// 计算出差值
        return (sum - dp[n][target]) - dp[n][target];
    }
}
```
#### 思考
总之上面这个题的核心思想就是将石头分成差不多大小的两堆，让其差别尽可能小。

#### 参考
1. [背包问题(0-1背包、完全背包、多重背包)详解](https://blog.csdn.net/huanghaocs/article/details/77920358)
2. [1049. Last Stone Weight II](https://blog.csdn.net/zjucor/article/details/90341208)



