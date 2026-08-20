---
date: 2025-11-26
tags:
  - 网格图
  - 动态规划
  - 数组
  - 模运算
  - 困难
---
[2435. 矩阵中和能被 K 整除的路径 - 力扣（LeetCode）](https://leetcode.cn/problems/paths-in-matrix-whose-sum-is-divisible-by-k/description/?envType=daily-question&envId=2025-11-26)
## 题目拆解 / 题意分析

[[不同路径]]升级版，在统计路径的同时需要记录当前路径和模 k 的情况

## 解题思路

[[网格图]][[动态规划|dp]] + [[模运算]]：

## 代码实现

```cpp
class Solution {
    static const int MOD = 1000000007;

public:
    int numberOfPaths(vector<vector<int>>& grid, int k) {
        int m = grid.size();
        int n = grid[0].size();
        vector dp(m + 1, vector(n + 1, vector<int>(k)));
        dp[0][1][0] = 1;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                for (int x = 0; x < k; x++) {
		            // 利用了模运算的奇妙性质
                    int pre_x = (x + grid[i][j]) % k;
                    dp[i + 1][j + 1][x] =
                        (dp[i][j + 1][pre_x] + dp[i + 1][j][pre_x]) % MOD;
                }
            }
        }
        return dp[m][n][0];
    }
};
```

## 复杂度分析

### 时间复杂度：**O(mnk)**

### 空间复杂度：**O(mnk)**

## 总结 / 错误反思

## 知识补充

