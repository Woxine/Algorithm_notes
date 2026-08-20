---
date: 2025-11-04
tags:
  - 递归
  - 二叉树
  - 回溯
  - dfs
  - 中等
---
[路径总和 II](https://leetcode.cn/problems/path-sum-ii/)
## 题目拆解 / 题意分析

没什么特别的，就是[[路径总和]]的升级版，用了回溯，同时需要记录具体路径

## 解题思路

递归 + dfs + 回溯：dfs 递归遍历整棵树，并记录路径；直到抵达叶子节点且路径和等于 targetSum，将路径添加到 ans；当离开当前节点时，触发回溯操作，路径列表始终保持为从根节点到当前节点的路径

## 代码实现

```cpp
class Solution {
public:
    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
        vector<vector<int>> ans;
	    // 全局变量 path 用于储存路径和回溯
        vector<int> path;
        auto dfs = [&](this auto&& dfs, TreeNode* node, int remain) -> void {
	        // 处理空节点
            if (!node) {
                return;
            }
            path.push_back(node->val);
            remain -= node->val;
            if (!node->left && !node->right && remain == 0) {
                ans.push_back(path);
            } else {
                dfs(node->left, remain);
                dfs(node->right, remain);
            }
            path.pop_back(); // 离开节点触发回溯
        };
        dfs(root, targetSum);
        return ans;
    }
};
```

## 复杂度分析

### 时间复杂度：**O($n^2$)**

主要开销来自节点数量和复制 path，其中 n 是二叉树的节点个数
对于「一条链 + 完全二叉树」这样的「扫帚型」二叉树，我们会在 O(n) 个叶子节点处，都去复制长为 O(n) 的 path，所以总的时间复杂度为 O($n^2$)

### 空间复杂度：**O(n)**

## 总结 / 错误反思

## 知识补充

