---
date: 2025-07-07
aliases:
  - Average value of a subarray with radius k
tags:
  - 滑动窗口
  - 数组
  - 中等
---
[半径为k的子数组平均值](https://leetcode.cn/problems/k-radius-subarray-averages/description/)

## 解题思路

```cpp
class Solution {
public:
    vector<int> getAverages(vector<int>& nums, int k) {
        vector<int> ans;
        int n = nums.size();
        long long sum = 0;
        for (int i = 0; i < n; i++){
            if (i < 2 * k){
                sum += nums[i];
                if (i - k < 0){
                    ans.push_back(-1);
                }
                continue;
            }
            sum += nums[i];
            ans.push_back((int)sum / (2 * k + 1));
            sum -= nums[i - 2 * k];
        }
        if (n > k){
            for (int j = 0; j < k; j++){
            ans.push_back(-1);
            }
        }
        return ans;
    }
};
```

以上是我第一次提交的错解，有几个问题：
- 填充-1的方式：我选择了手动填充，但其实可以在初始化时将`ans`的元素全设为-1，相当于默认为-1。以后遇到这种不简洁，很复杂的填充方式可以多想想其他方法。
- 最开始声明`sum`的时候用的是`int`，忽略了数据范围

```cpp
class Solution {
public:
    vector<int> getAverages(vector<int>& nums, int k) {
        int n = nums.size(), index = 2 * k + 1;
        vector<int> ans(n, -1); // 初始化默认为-1
        long long sum = 0; // 防止数据溢出
        for (int i = 0; i < n; i++){
            sum += nums[i];
            if (i < index - 1){
                continue;
            }
            ans[i - k] = sum / index;
            sum -= nums[i - index + 1];
        }
        return ans;
    }
};
```

## 总结

虽然与前两道题略有不同，但是总体上符合[[滑动窗口]]的套路