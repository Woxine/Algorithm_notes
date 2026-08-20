---
date: 2025-11-04
tags:
  - 递归
  - 二叉树
  - 哈希表
  - 回溯
  - 前缀和
  - dfs
  - 中等
---
- [路径总和 III](https://leetcode.cn/problems/path-sum-iii/)
## 题目拆解 / 题意分析

与[[路径总和 II]]不同，这一次，路径的起点和终点都不确定，唯一确定的是它仍然是连续的。所以我们要求的就是[[二叉树]]中满足特殊要求的连续节点的和

听起来很耳熟？没错！这跟”求满足要求的连续子数组的和“是一个道理——[[和为 K 的子数组]]

## 解题思路

[[递归]] + dfs + [[哈希表]] + [[前缀和]] + [[回溯]]：在 dfs 递归遍历[[二叉树]]路径终点时，统计有多少满足条件的起点；在退出终点时回溯

## 代码实现

```cpp
class Solution {
public:
    int pathSum(TreeNode* root, int targetSum) {
        int ans = 0;
        // 初始化cnt[0] = 1, 相当于前缀和中s[0] = 0
        unordered_map<long long, int> cnt = {{0, 1}};
        auto dfs = [&](this auto&& dfs, TreeNode* node, long long sum) -> void {
            if (!node) {
                return;
            }
            // 先更新再查询
            sum += node->val;
            ans += cnt[sum - targetSum];
            cnt[sum]++;
            dfs(node->left, sum);
            dfs(node->right, sum);
            cnt[sum]--; // 回溯
        };
        dfs(root, 0);
        return ans;
    }
};
```

## 复杂度分析

### 时间复杂度：**O(n)**

### 空间复杂度：**O(n)**

## 总结 / 错误反思

与[[和为 K 的子数组]]不同，这道题是”先更新再查询“，而不是”先查询再更新“。因为这道题遍历的是”满足条件的终点“，要先把终点的节点值加上才能开始统计，不然当`targetSum = 0`时，可能出现长度为 0 的路径（节点本身）；在[[和为 K 的子数组]]中，统计的是”满足条件的左端点的数量“，所以在更新答案之前，自然不能将当前端点（右端点）加入左端点中

## 知识补充

