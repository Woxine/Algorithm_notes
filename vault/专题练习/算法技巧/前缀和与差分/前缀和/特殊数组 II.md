---
date: 2025-08-20
tags:
  - 前缀和
  - 数组
  - 中等
---
[特殊数组 II](https://leetcode.cn/problems/special-array-ii/)
## 解题思路

要查询一个**子数组**是否是“特殊数组”，为什么会想到[[前缀和]]呢？

之前提到过，前缀和就是在为“**对容器中元素的同一操作进行存档**”，相似的，也可以“**为容器中元素的状态/性质进行记录**”，例如从月初开始**每天**记录“这个月一共**锻炼了多少天**”，每天的**状态**就是锻炼了/没锻炼，如果我想知道从15号到20号（**某一区间**）有多少天在锻炼，就只需要将20号的记录减去14号的记录就行了

这道题记录的不是单个元素的状态/性质，而是相邻的一对元素

```cpp
class Solution {
public:
    vector<bool> isArraySpecial(vector<int>& nums,
                                vector<vector<int>>& queries) {
        int q = queries.size(), n = nums.size();
        vector<int> profix(n + 1);
        for (int i = 1; i < n; i++) {
	        // 记录状态
            profix[i] = profix[i - 1] + (nums[i - 1] % 2 == nums[i] % 2);
        }
        vector<bool> ans(q);
        for (int i = 0; i < q; i++) {
            auto& q = queries[i];
            // 表示区间内没有状态更新，即没有出现奇偶性相同的两个数字
            ans[i] = profix[q[0]] == profix[q[1]];
        }
        return ans;
    }
};
```

## 复杂度分析

### 时间复杂度：**O(max(n, q))**

### 空间复杂度：**O(max(n + 1, q))**

## 新知识点

## 总结

