---
date: 2025-08-14
tags:
  - 枚举
  - 数组
  - 中等
---
[元素和最小的山形三元组 II](https://leetcode.cn/problems/minimum-sum-of-mountain-triplets-ii/)
## 解题思路

- [[枚举中间]]：想要得到最小的三元组，我们可以枚举中间值`nums[j]`，将其与它左右两边的最小值比较，找到符合条件的最小和。那么怎么得到两边的最小值呢？左边的最小值能够在枚举的过程中得到，而右边的最小值可以通过预处理得到

```cpp
class Solution {
public:
    int minimumSum(vector<int>& nums) {
        int n = nums.size();
        // suf[i] 存储从 i 到 n - 1 中的最小值
        vector<int> suf(n);
        suf[n - 1] = nums[n - 1];
        for (int i = n - 2; i > 1; i--) {
	        // i 到 n - 1 中的最小值
	        // 通过递推，就是 i + 1 到 n - 1 中最小值与当前值之间的最小值
            suf[i] = min(suf[i + 1], nums[i]);
        }
        int ans = INT_MAX, pre = nums[0];
        for (int j = 1; j < n - 1; j++) {
            if (pre < nums[j] && nums[j] > suf[j + 1]) {
                ans = min(ans, pre + nums[j] + suf[j + 1]);
            }
            pre = min(pre, nums[j]);
        }
        return ans == INT_MAX ? -1 : ans;
    }
};
```

## 复杂度分析

### 时间复杂度：**O(n)**

### 空间复杂度：**O(n)**

## 新知识点

## 总结

