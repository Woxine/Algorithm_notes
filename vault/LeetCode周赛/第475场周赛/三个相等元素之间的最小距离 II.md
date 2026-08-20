---
date: 2025-11-09
tags:
  - 队列
  - 哈希表
  - 数学
  - 数组
  - 中等
---
[3741. 三个相等元素之间的最小距离 II](https://leetcode.cn/problems/minimum-distance-between-three-equal-elements-ii/)
## 题目拆解 / 题意分析

从数学意义上讲，`abs(i - j) + abs(j - k) + abs(k - i)`这个式子就是把三个相同元素的下标相互之间的距离加起来了，想要这个结果尽可能小，那么这三个元素应当尽可能近，我一开始就是这么想的

所以自然而然地想到用滑动窗口解，维护一个尽可能小的窗口，每次更新窗口端点时，更新 ans

然后没写出来

于是我干脆用一个[[哈希表]] + [[队列]]来存储相等元素的下标信息，直接模拟整个过程，就有了第一个解法，但是巨慢，不过好歹是过了

但实际上当我从数学意义的角度思考时，已经很接近官方答案了；将这三个下标放在数轴上就能很直观的发现，`abs(i - j) + abs(j - k) + abs(k - i) = 2 * ans(k - i)`，所以直接在[[哈希表]]中套个`vector`，记录每个相等元素出现的位置，然后遍历 k 和 i 就行

## 解题思路

- [[哈希表]] + [[队列]]：顺序遍历整个数组，将相同元素的下标压入哈希表中对应的队列，当队列元素等于 3 时，更新一次 ans，并将最左边的元素出队
- [[哈希表]] + vector：按照相同元素分组，记录相同元素的下标，保存到列表中；遍历表中元素得到答案

## 代码实现

- 方法一：

```cpp
class Solution {
public:
    int minimumDistance(vector<int>& nums) {
        int n = nums.size();
        if (n <= 2) {
            return -1;
        }
        int ans = INT_MAX;
        map<int, queue<int>> positions;
        for (int m = 0; m < n; m++) {
            auto& pos = positions[nums[m]];
            pos.push(m);
            if (pos.size() == 3) {
                int i = pos.front();pos.pop();
                int j = pos.front();
                int k = pos.back();
                ans = min(ans, abs(i - j) + abs(j - k) + abs(k - i));
            }
        }
        return ans == INT_MAX ? -1 : ans;
    }
};
```

- 方法二：

```cpp
class Solution {
public:
    int minimumDistance(vector<int>& nums) {
        int n = nums.size();
        int ans = INT_MAX;
        unordered_map<int, vector<int>> pos;

        for (int i = 0; i < n; i++) {
            pos[nums[i]].emplace_back(i);
        }

        for (auto& [_, p] : pos) {
            for (int i = 2; i < p.size(); i++) {
                ans = min(ans, (p[i] - p[i - 2]) * 2);
            }
        }
        return ans == INT_MAX ? -1 : ans;
    }
};
```

## 复杂度分析

### 时间复杂度：**O(nlogm) / O(n)**

其中n是数组长度，m是不同数字的数量。
方法一在最坏情况下m = n，时间复杂度为 O(n log n)，多出来的部分是 map 的开销
方法二为 O(n)

### 空间复杂度：**O(n)**

## 总结 / 错误反思

还是要尝试通过理解数学性质来简化题目，最好是在草稿本上写写画画，要是画个数轴说不定就看出来了，不过比赛的时候没有想这么多就是了

一开始想用滑窗，整了十多分钟没整出来

## 知识补充

