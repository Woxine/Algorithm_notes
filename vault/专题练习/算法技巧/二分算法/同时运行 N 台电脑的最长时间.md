---
date: 2025-12-02
tags:
  - 二分算法
  - 数组
  - 贪心算法
  - 排序
  - 困难
---
[2141. 同时运行 N 台电脑的最长时间 - 力扣（LeetCode）](https://leetcode.cn/problems/maximum-running-time-of-n-computers/description/)
## 题目拆解 / 题意分析

要让 n 台电脑同时运行，就是要 n 个 电池同时有电；这些电池中肯定有电量充足的“大电池”和电量相对较少的“小电池”，记电池电量和为 sum，则理论上至多可以供电 x = $⌊sum/n​⌋$ 分钟，所以我们可以贪心地认为就是可以供电 x 分钟；然后我们按电池电量对电池进行排序，倒序遍历

- 如果当前电池电量大于等于 x，说明它可以用 x 分钟，我们就只要关心剩下的电池能不能供 n - 1 台电脑运行 x' = $⌊sum-cur/n-1​⌋$分钟；
- 反之，说明剩下的所有电池容量不大于 x'，满足：$n' * ⌊sum'/n'​⌋<=sum'$，此时的 x' 就是答案

同时我们可以看出，x 越小，电量越富余，越容易达成；x 越大，需要的电量越多，越难达成，**具有单调性**，所以可以用[[二分算法]]猜答案

## 解题思路

二分答案：

[[贪心算法]]：

## 代码实现

二分答案：

```cpp
class Solution {
public:
    long long maxRunTime(int n, vector<int>& batteries) {
        long long sum = reduce(batteries.begin(), batteries.end(), 0LL);
        // 设置左右边界，不可能达到
        long long l = 0, r = sum / n + 1;
        while (l + 1 < r) {
            long long mid = l + (r - l) / 2;  // 表示本轮猜的x
            long long cnt = 0;  // 记录电池可用总量
            for (long long b : batteries) {
                cnt += min(b, mid);  // 电量再多也只能运行x分钟
            }
            // 如果电池可用总量大于等于电脑运行时间
            // 说明猜小了，l 更新为 mid(x)
            (n * mid <= cnt ? l : r) = mid;
        }
        //  求最大，返回向右更新的那个
        return l;
    }
};
```

贪心：

```cpp
class Solution {
public:
    long long maxRunTime(int n, vector<int>& batteries) {
        long long sum = reduce(batteries.begin(), batteries.end(), 0LL);
        ranges::sort(batteries, greater());
        for (int& b : batteries) {
            if (b <= sum / n) {
                return sum / n;
            }
            sum -= b;
            n--;
        }
        return sum / n;
    }
};
```

## 复杂度分析

### 时间复杂度：**O(mlog(S/n))/O(mlogm)**

- 其中 m 是 batteries 的长度，S 是 batteries 的元素和，开销主要来自求和与二分；
- 主要开销来自求和，排序和遍历

### 空间复杂度：**O(1)**

## 总结 / 错误反思

## 知识补充

