---
date: 2025-12-03
tags:
  - 背包问题
  - 动态规划
  - 数组
  - 中等
---
[零钱兑换 II](https://leetcode.cn/problems/coin-change-ii/)
## 题目拆解 / 题意分析

这道题其实和[[组合总和 Ⅳ]]非常像，只不过一个求得组合数，一个求的排列数

## 解题思路

## 代码实现

```cpp
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        vector<unsigned> dp(amount + 1);
        dp[0] = 1;
        for (int& c : coins) {
            for (int j = c; j <= amount; j++) {
                dp[j] += dp[j - c];
            }
        }
        return dp[amount];
    }
};
```

## 复杂度分析

### 时间复杂度：**O(c·amount)**

### 空间复杂度：**O(amount)**

## 总结 / 错误反思

## 知识补充

