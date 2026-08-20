---
date: 2025-11-03
tags:
  - 哈希表
  - 前缀和
  - 数组
  - 简单
---
[和为 K 的子数组](https://leetcode.cn/problems/subarray-sum-equals-k/)
## 题目拆解 / 题意分析

涉及到 “连续子数组和” 的问题应当自然地想到[[前缀和]]，根据题意可以得到式子：`s[j] - s[i] = k`变形以下就能得到：`s[j] = s[i] + k`

看起来很眼熟？是了！[1. 两数之和](https://leetcode.cn/problems/two-sum/description/?envType=study-plan-v2&envId=top-100-liked)就是在干一件类似的事

本质上就是 [[枚举右，维护左]]
## 解题思路

[[前缀和]] + [[哈希表]] + 枚举：枚举`s[j]`，统计有多少`s[i]`满足上述式子，用哈希表存起来

## 代码实现

```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
	    // 由于不需要查询，只是存储前缀和，所以用 sum 就行
        int sum = 0, ans = 0;
        unordered_map<int, int> cnt;
        for (int& n : nums) {
            cnt[sum]++;
            sum += n;
            ans += cnt[sum - k];
        }
        return ans;
    }
};
```

## 复杂度分析

### 时间复杂度：**O(n)**
### 空间复杂度：**O(n)**

## 总结 / 错误反思

前缀和与哈希表常常和 “枚举右，维护左” 结合起来，前者负责计数，后者负责维护前者。关键在于找到联系左与右的式子，接下来我们将考察一个更复杂的例子：[[边界与内部和相等的稳定子数组]]

## 知识补充

