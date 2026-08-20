---
date: 2025-08-08
tags:
  - 二分答案
  - 数组
  - 逆向思维
  - 中等
---
[标记所有下标的最早秒数 I](https://leetcode.cn/problems/earliest-second-to-mark-indices-i/)
## 解题思路

这道题理解起来有点困难，弯弯绕绕的。我最开始想的是，遍历`changeIndices`中的元素，如果为零就标记，否则就减一，但实际上这种理解是错误的。然后我在其他方向上做了些努力，最终都失败了。于是我参考了灵神的题解，得到了答案

这道题可以这么理解：

	你有 n 门课程需要考试，第 i 门课程需要用 nums[i] 天复习。同一天只能复习一门课程。
	在第 i 天，你可以选择参加第 changeIndices[i] 门课程的考试。考试这一天不能复习。
	搞定所有课程的复习+考试，至少要多少天？

这下就清楚了，天数越多，成功率越高，**具有单调性**，可以用[[二分算法]]；同时，**目标很清晰**，就是：“**搞定所有课程的复习+考试，至少要多少天？**”，所以我们实际上不用关心具体的操作，只需要判断：

- 是否考完所有考试
- 是否够时间复习

因为考试前必须复习完，所以如果在**可以考试的日期最后一次出现之前**都没复习完，就失败了。于是我们可以逆向遍历`changeIndices`中的元素，如果第一次遇到`changeIndices[i]`，就直接考试（标记），然后将需要复习的天数记录下来；否则这天用来复习，把“要复习的天数”减一（前提是还有时间）

```cpp
class Solution {
public:
    int earliestSecondToMarkIndices(vector<int>& nums,
                                    vector<int>& changeIndices) {
        int n = nums.size(), m = changeIndices.size();
        // 根据题目要求确定上下界
        int left = 0, right = m + 1;
        // 新开一个数组记录考试时间
        vector<int> done(n);
        while (left + 1 < right) {
            int mid = (left + right) / 2;
            (check(nums, changeIndices, done, mid, n) ? right : left) = mid;
        }
        // 检验结果是否合法
        return right <= m ? right : -1;
    }

    bool check(const vector<int>& nums, const vector<int>& changeIndices,
               vector<int>& done, int mid, int n) {
        // 初始化一共需要参加的考试和需要复习的天数
        int exam = n, study = 0;
        // 从目标天数开始逆向遍历
        for (int i = mid - 1; i >= 0 && study <= i + 1; i--) {
	        // 题目下标从1开始，所以要减一
            int idx = changeIndices[i] - 1;
            // 这里写 if (done[idx] != mid) 是因为 mid 每次 check 都会变
            // 这样即使每次 check 都用同一个数组来标记也不会重复
            // 这样就省去的每次 check 都新开数组的花销
            // if (done[idx] != mid){
		    //    done[idx] = 1;
		    //  ...
            //} 这样就不行
            if (done[idx] != mid) {
                done[idx] = mid;
                exam--;
                study += nums[idx];
            } else if (study) {
                study--;
            }
        }
        // 如果最后考试全考完，也没有额外要复习的天数，就算成功
        return exam == 0 && study == 0;
    }
};
```
## 复杂度分析

### 时间复杂度：**O(mlogm)**

其中 m 为 changeIndices 的长度。二分的时候保证 n≤m，时间复杂度以 m 为主

### 空间复杂度：**O(n)**

其中 n 为 nums 的长度

## 新知识点

## 总结

这道题很好地体现了[[逆向思维]]的适用场景：
- 正难则反
- 题目的目标**非常明确**，但是要达到这个目标的过程有点复杂

