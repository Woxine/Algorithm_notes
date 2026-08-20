---
date: 2025-09-13
tags:
  - 堆
  - 数组
  - 优先队列
  - 中等
---
[数据流中的第 K 大元素](https://leetcode.cn/problems/kth-largest-element-in-a-stream/)
## 解题思路

优先队列（[[堆]]）：“第 k 大的数” ，就是 “最大的 k 个数中最小的那一个”，根据堆序关系，我们只需要构建一个小根堆，然后每次取出堆顶的元素即可

```cpp
class KthLargest {
private:
	// 构建小根堆
    priority_queue<int, vector<int>, greater<>> min_heap;
    int k;

public:
    KthLargest(int k, vector<int>& nums) {
        this->k = k;
        // 遍历数组
        for (int num : nums) {
            add(num);
        }
    }
	// 核心函数
    int add(int val) {
	    // 不断将元素加入堆
        min_heap.push(val);
        // 维护堆的大小为 k，保证堆顶元素最小
        if (min_heap.size() > k){
            min_heap.pop();
        }
        return min_heap.top();
    }
};

/**
 * Your KthLargest object will be instantiated and called as such:
 * KthLargest* obj = new KthLargest(k, nums);
 * int param_1 = obj->add(val);
 */
```

## 复杂度分析

### 时间复杂度：**O(nlogk)**

其中 n 为初始化时 nums 的长度

### 空间复杂度：**O(k)**

## 新知识点

## 总结

