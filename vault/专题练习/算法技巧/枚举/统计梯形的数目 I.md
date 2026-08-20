---
date: 2025-08-12
tags:
  - 哈希表
  - 枚举
  - 数组
  - 中等
---
[统计梯形的数目 I](https://leetcode.cn/problems/count-number-of-trapezoids-i/)
## 解题思路

我们先固定一条边，然后枚举与其平行的边，用[[哈希表]]记录每条边上点的数量，梯形数量相当于在这两条平行线上的点中分别选两个点，再根据乘法原理累加结果

```cpp
class Solution {
public:
    int countTrapezoids(vector<vector<int>>& points) {
	    // 记录每条边上点的数量
        unordered_map<int, int> map;
        for (auto& p : points) {
            map[p[1]]++;
        }
        long long ans = 0, sum = 0;
        for (auto& [_, c] : map) {
	        // 在当前边上选两个点（枚举右）
            long long k = (long long)c * (c - 1) / 2;
            // 乘法原理累加
            ans += sum * k;
            // 计入点数总和（维护左）
            sum += k;
        }
        return ans % 1000000007;
    }
};
```

## 复杂度分析

### 时间复杂度：**O(n)**

### 空间复杂度：**O(n)**

## 新知识点

哈希表迭代器写法：
```cpp
for (auto& [_, c] : map) // 这里取的是值
```
## 总结

