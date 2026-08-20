---
date: 2025-12-09
tags:
  - 贪心算法
  - 数组
  - 排序
  - 堆
---
[3767. 选择 K 个任务的最大总分数 - 力扣（LeetCode）](https://leetcode.cn/problems/maximize-points-after-choosing-k-tasks/)
## 题目拆解 / 题意分析

一开始用动态规划写的，爆内存了；然后优化空间，结果超时了
同时考虑两个数组是很困难的，想想办法把两个数组合在一起；对于每个任务，要么选1，要么选2，所以最终增加的分值就是1和2的分值差

## 解题思路

[[贪心算法]]：贪心地累加最大的正差值，然后保证有k个任务选1

## 代码实现

```cpp
class Solution {
public:
    long long maxPoints(vector<int>& technique1, vector<int>& technique2,
                        int k) {
        int n = technique1.size();
        long long ans = 0;
        vector<int> diff;
        for (int i = 0; i < n; i++) {
            ans += technique1[i];  // 全选 1
            int d = technique2[i] - technique1[i];  // 记录2比1多的部分
            if (d > 0) {
                diff.emplace_back(d);
            }
        }
        sort(diff.begin(), diff.end(), greater<int>());
        int limit = min(n - k, (int)diff.size());
        // 加上多出来的部分
        ans += reduce(diff.begin(), diff.begin() + limit, 0LL);
        return ans;
    }
};
```

## 复杂度分析

### 时间复杂度：**O(nlogn)**

### 空间复杂度：**O(n)**

## 总结 / 错误反思

对于 n ≤ $10^5$ 这个数据量，O(nlogn) 是比较合适的，O($n^2$) 就会超时

## 知识补充

