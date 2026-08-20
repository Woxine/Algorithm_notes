---
date: 2025-08-08
tags:
  - 二分答案
  - 数组
  - 中等
---
[H 指数 II](https://leetcode.cn/problems/h-index-ii/)
## 解题思路

我们需要找到一个最大的 h 使得 至少有 h 篇论文被引用了 h 次，而且数组已经排好序了。同时，如果至少有 h 篇论文被引用了 h 次，那么一定至少有 h - 1 篇论文被引用了 h - 1次；如果没有有 h 篇论文被引用了 h 次，那么一定没有有 h + 1篇论文被引用了 h + 1次，**具有单调性**，可以用[[二分算法]]

因为论文的引用次数是从小到大排的，所以要从后面开始数才对，于是我们对 h 的大小进行二分，尝试找到**最大值**

```cpp
class Solution {
public:
    int hIndex(vector<int>& citations) {
        int n = citations.size();
        int left = 0, right = n + 1;
        while (left + 1 < right) {
            int mid = left + (right - left) / 2;
            (citations[n - mid] >= mid ? left : right) = mid;
        }
        return left;
    }
};
```
## 复杂度分析

### 时间复杂度：**O(logn)**

其中 n 是 citations 的长度，每次二分将范围减半，所以是O(logn)

### 空间复杂度：**O(1)**

## 新知识点

## 总结

这道题求的是**最大值**，也就是说满足条件的值都在范围内较小的那一部分，也就是左端点外，我们需要找到这之中最大的那个，而右端点之外的值都不满足条件；当我们求**最小值**，也就是说满足条件的值都在范围内较大的那一部分，情况是反过来的

这就是为什么求最大值时返回的是 left ；求最小值时返回的是 right 
