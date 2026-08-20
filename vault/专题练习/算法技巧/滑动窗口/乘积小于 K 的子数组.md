---
date: 2025-08-04
tags:
  - 滑动窗口
  - 数组
  - 中等
---
[乘积小于 K 的子数组](https://leetcode.cn/problems/subarray-product-less-than-k/)
## 解题思路

要找到**符合要求**的**连续子数组**，用[[滑动窗口]]就很合适，这道题是**越短越合法**类型的一道例题，详见[[滑动窗口]]

```cpp
class Solution {
public:
    int numSubarrayProductLessThanK(vector<int>& nums, int k) {
        int ans = 0, mult = 1, l = 0;
        if (k <= 1) {
            return 0;
        }
        for (int r = 0; r < nums.size(); r++) {
            mult *= nums[r];
            while (mult >= k) {
                mult /= nums[l++];
            }
            ans += r - l + 1;
        }
        return ans;
    }
};
```

## 复杂度分析

### 时间复杂度：**O(n)**

其中 `n` 为 `nums` 的长度。虽然写了个二重循环，但是内层循环中对 `left` 加一的**总**执行次数不会超过 `n` 次，所以总的时间复杂度为 **O(n)**

### 空间复杂度：**O(1)**

仅用到若干额外变量

## 新知识点

## 总结

### 越短越合法

一般要写 `ans += right - left + 1`。

内层循环结束后，`[left,right]` 这个子数组是满足题目要求的。由于子数组越短，越能满足题目要求，所以除了 `[left,right]`，还有 `[left+1,right],[left+2,right],…,[right,right]` 都是满足要求的。也就是说，当右端点固定在 right 时，左端点在 `left,left+1,left+2,…,right`的所有子数组都是满足要求的，这一共有 `right−left+1` 个。