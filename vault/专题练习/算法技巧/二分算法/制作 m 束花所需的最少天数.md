---
date: 2025-08-07
tags:
  - 二分答案
  - 数组
  - 中等
---
[制作 m 束花所需的最少天数](https://leetcode.cn/problems/minimum-number-of-days-to-make-m-bouquets/)
## 解题思路

天数越长，开的花越多，**具有单调性**，可以用[[二分算法]]。我们需要知道，在某一天时，同时有多少相邻的花正在开，进而算出它们是否足够制成目标花束。

```cpp
class Solution {
public:
    int minDays(vector<int>& bloomDay, int m, int k) {
	    // 特判，注意数据溢出
        if ((long long)m * k > bloomDay.size()) {
            return -1;
        }
        auto check = [&](int mid) {
	        // f_left 记录还需要多少花束
	        // cur_k 记录当前相邻的盛开的花的数量
            int f_left = m, cur_k = 0;
            for (int isbloom : bloomDay) {
	            // 如果当前花开了，记录下来，否则有花不相邻，cur_k 清空
                cur_k = (isbloom <= mid ? cur_k + 1 : 0);
                // 如果能得到一束花，更新状态
                if (cur_k == k) {
                    f_left--;
                    cur_k = 0;
                }
            }
            return f_left <= 0 ? true : false;
        };
        int left = 0, right = ranges::max(bloomDay);
        while (left + 1 < right) {
            int mid = left + (right - left) / 2;
            // 花束的数量够，减少天数；不够就增加
            (check(mid) ? right : left) = mid;
        }
        return right;
    }
};
```

## 复杂度分析

### 时间复杂度：**O(n × log(max_d))***

1. **单次 check 的时间复杂度**：  
    仅**一次遍历**数组`bloomDay`，每个元素只访问 1 次，为 **O(n)**。
2. **二分查找的次数**：  
    查找范围是`[min_d-1, max_d]`，最多循环次数为 **O(log(max_d))**

### 空间复杂度：**O(1)**

## 新知识点

## 总结

