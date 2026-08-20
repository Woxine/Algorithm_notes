---
date: 2025-09-13
tags:
  - 暴力模拟
  - 堆
  - 数组
  - 优先队列
  - 简单
---
[超过阈值的最少操作数 II](https://leetcode.cn/problems/minimum-operations-to-exceed-threshold-value-ii/)
## 解题思路

优先队列（[[堆]]）纯模拟：

```cpp
class Solution {
public:
    int minOperations(vector<int>& nums, int k) {
        int ans = 0;
        // 创建小根堆
        priority_queue<long long, vector<long long>, greater<>>
	    min_heap(nums.begin(),nums.end());
	    // 模拟，注意数值溢出
        while (min_heap.top() < k) {
            long long x = min_heap.top(); min_heap.pop();
            long long y = min_heap.top(); min_heap.pop();
            // 优先队列已经保证了元素之间的大小关系
            min_heap.push(x * 2 + y);
            ans++;
        }
        return ans;
    }
};
```

## 复杂度分析

### 时间复杂度：**O(nlogn)**

其中 n 为 nums 的长度。由于每次循环都会出堆一个元素，所以循环次数是 O(n) 的。每次操作堆需要 O(logn) 的时间

### 空间复杂度：**O(n)**

## 新知识点

`std::priority_queue` 的模板定义如下，其中第三个参数 `Compare` 用于指定比较规则：

```cpp
template <
    class T,                           // 元素类型
    class Container = std::vector<T>,  // 底层容器（默认vector）
    class Compare = std::less<T>       // 比较器（默认大根堆）
> class priority_queue;
```

- 默认情况下，`Compare` 为 `std::less<T>`，此时 `priority_queue` 是**大根堆**
- 若将 `Compare` 指定为 `std::greater<T>`，则 `priority_queue` 会成为**小根堆**
## 总结

