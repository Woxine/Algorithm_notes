---
date: 2025-09-01
tags:
  - 差分
  - 数组
  - 中等
---
[零数组变换 I](https://leetcode.cn/problems/zero-array-transformation-i/)
## 解题思路

差分数组例题，纯模拟：

```cpp
class Solution {
public:
    bool isZeroArray(vector<int>& nums, vector<vector<int>>& queries) {
        int n = nums.size();
        vector<int> diff(n + 1);
        for (auto& q : queries) {
            diff[q[0]]--;
            diff[q[1] + 1]++;
        }
        int sum = 0;
        for (int i = 0; i < n; i++) {
            sum += diff[i];
            if (sum + nums[i] > 0) {
                return false;
            }
        }
        return true;
    }
};
```

## 复杂度分析

### 时间复杂度：**O(n)**

### 空间复杂度：**O(n)**

## 新知识点

## 总结

