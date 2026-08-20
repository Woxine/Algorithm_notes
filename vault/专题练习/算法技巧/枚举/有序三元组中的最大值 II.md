---
date: 2025-08-13
tags:
  - 数组
  - 枚举
  - 中等
---
[有序三元组中的最大值 II](https://leetcode.cn/problems/maximum-value-of-an-ordered-triplet-ii/)
## 解题思路

- [[枚举右，维护左]]：这种三变量问题就需要维护两个变量了，但是总之，想要结果足够大，就要保证`nums[i]−nums[j]`和`nums[k]`最大，所以我们可以：
	- 枚举 k ，然后维护一个最大的`nums[i]−nums[j]`，这个过程中要保证`nums[i]−nums[j]`最大有需要维护一个最大的`nums[i]`
	- 枚举 j ，然后维护一个最大的 `nums[i]`和`nums[k]`

这里枚举 k ：

```cpp
class Solution {
public:
    long long maximumTripletValue(vector<int>& nums) {
        long long ans = 0;
        int maxd = 0, maxm = 0;
        for (int n : nums) {
	        // 首先维护 ans，这样就能保证当前枚举的值下标最大，因为 n 是新出现的
	        // 此时 n 还是“右”，下一步将要将其放进“左”中维护
            ans = max(ans, (long long)maxd * n);
            // ans 已经更新完了，所以此时的 k 可以视作已经移动到下一位了
            // 于是 n 相当于是新出现的 nums[j]
            maxd = max(maxd, maxm - n);
            // 此时最大差值也已经更新完了，所以此时的 j 可以视作已经移动到下一位了
            // 最后再来更新最大的 nums[i]，这样就能天然地保证 i < j < k
            maxm = max(maxm, n);
        }
        return ans;
    }
};
```

## 复杂度分析

### 时间复杂度：**O(n)**

### 空间复杂度：**O(1)**

## 新知识点

## 总结

这里变量的维护顺序就是保证 i < j < k 的关键
