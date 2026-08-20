---
date: 2025-09-13
tags:
  - 堆
  - 优先队列
  - 数组
  - 中等
---
[第 K 近障碍物查询](https://leetcode.cn/problems/k-th-nearest-obstacle-queries/)
## 解题思路

优先队列（堆）：这道题跟[[数据流中的第 K 大元素]]很像，稍作修改就行；同时这道题的预处理手法也用过[[半径为k的子数组平均值]]，但是这次忘了

```cpp
class Solution {
private:
    vector<int> ans;
    priority_queue<int> pq;

public:
    vector<int> resultsArray(vector<vector<int>>& queries, int k) {
        for (auto& q : queries) {
            int val = abs(q[0]) + abs(q[1]);
            pq.push(val);
            if (pq.size() < k) {
                ans.emplace_back(-1);
                continue;
            }
            if (pq.size() > k) {
                pq.pop();
            }
            ans.emplace_back(pq.top());
        }
        return ans;
    }
};
```

优化后：

```cpp
class Solution {
public:
    vector<int> resultsArray(vector<vector<int>>& queries, int k) {
        vector<int> ans(queries.size(), -1);
        priority_queue<int> pq;
        for (int i = 0; i < queries.size(); i++) {
            pq.push(abs(queries[i][0]) + abs(queries[i][1]));
            if (pq.size() > k) {
                pq.pop();
            }
            if (pq.size() == k) {
                ans[i] = pq.top();
            }
        }
        return ans;
    }
};
```

## 复杂度分析

### 时间复杂度：**O(nlogk)**

其中 n 是 queries 的长度

### 空间复杂度：**O(k)**

## 新知识点



## 总结

