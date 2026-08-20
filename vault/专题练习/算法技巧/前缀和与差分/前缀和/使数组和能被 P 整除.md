---
date: 2025-11-30
tags:
  - 前缀和
  - 哈希表
  - 数组
  - 模运算
  - 枚举
  - 中等
---
[1590. 使数组和能被 P 整除 - 力扣（LeetCode）](https://leetcode.cn/problems/make-sum-divisible-by-p/description/?envType=daily-question&envId=2025-11-30)
## 题目拆解 / 题意分析

其实就是 nums 的元素和 与 去掉的子数组的和 **同余**，而我们看到**连续子数组求和**，很容易就能想到前缀和，然后用哈希表维护总和与前缀和之间的关系

## 解题思路

[[枚举右，维护左]] + [[哈希表]] + [[前缀和]]：

## 代码实现

```cpp
class Solution {
public:
    int minSubarray(vector<int>& nums, int p) {
        int n = nums.size();
        int ans = n, s = 0;
        int x = reduce(nums.begin(), nums.end(), 0LL) % p;
        unordered_map<int, int> pos{{s, -1}};
        for (int i = 0; i < n; i++) {
            s = (s + nums[i]) % p;
            pos[s] = i;
            auto it = pos.find((s - x + p) % p);  // 保证为非负数
            if (it != pos.end()) {
                ans = min(ans, i - it->second);
            }
        }
        return ans < n ? ans : -1;
    }
};
```

## 复杂度分析

### 时间复杂度：**O(n)**

### 空间复杂度：**O(n)**

## 总结 / 错误反思

能够在求前缀和的过程中也进行模运算

## 知识补充

