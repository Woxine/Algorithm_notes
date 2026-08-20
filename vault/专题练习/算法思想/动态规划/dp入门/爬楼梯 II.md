---
date: 2025-11-03
tags:
  - 动态规划
  - 记忆化搜索
  - 中等
---
[爬楼梯 II](https://leetcode.cn/problems/climbing-stairs-ii/)
## 题目拆解 / 题意分析

这道题更是[[使用最小花费爬楼梯]]的升级版，但本质还是[[爬楼梯]]，花费从"由哪里开始要花多少"变成了”到哪里要花多少“，然后跳跃阶数多了一个选择。记`dp[i]`为”跳到第 i 级的花费“，状态转移方程就是：`dp[i] = min(dp[i - 1] + 1, dp[i - 2] + 4, dp[i - 3] + 9) + cost[i]`

## 解题思路

[[动态规划]]：依然是递推 + 空间优化，只不过多了一个变量

## 代码实现

```cpp
class Solution {
public:
    int climbStairs(int n, vector<int>& costs) {
	    // f0~3 分别表示上一个，上上个和上上上个状态
        int f0 = 0, f1 = 0, f2 = 0;
        for (auto& c : costs) {
            int dp = min({f0 + 1, f1 + 4, f2 + 9}) + c;
            f2 = f1;
            f1 = f0;
            f0 = dp;
        }
        return f0;
    }
};
```

## 复杂度分析

### 时间复杂度：**O(n)**

O(nK)，其中 K=3 是最多可以跳的台阶数

### 空间复杂度：**O(K) / O(1)**

## 总结 / 错误反思

这道题进一步展示了动态规划的核心：**最优子问题**和**状态转移**，最终答案总是由上一个状态加一个选择得到的，不管子问题有多少个——见[[组合总和 Ⅳ]]
## 知识补充

在`int dp = min({f0 + 1, f1 + 4, f2 + 9}) + c;`中

对于 `min({a, b, c})` 它不是常数时间的简单比较，而是隐藏了容器构造与函数调用的开销。会被编译成如下：

```cpp
std::initializer_list<int> tmp = {a, b, c};
int res = *std::min_element(tmp.begin(), tmp.end());
```

而 `min(a, min(b, c))` 被编译器优化为纯粹的内联整数比较，没有任何容器创建，这是 O(1) 的原生操作。

```cpp
int res = (a < (b < c ? b : c) ? a : (b < c ? b : c));
```

所以改成下式会快一些：

```cpp
int dp = min(min(f0 + 1, f1 + 4), f2 + 9) + c;
```