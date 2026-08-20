---
date: 2025-11-11
tags:
  - 递归
  - 网格图
  - dfs
  - 数组
  - 中等
---
[LCS 03. 主题空间](https://leetcode.cn/problems/YesdPw/)
## 题目拆解 / 题意分析

[[岛屿的最大面积]]的变种，多了两个环节：判断是否与走廊相邻；判断是否为同一岛屿类型

## 解题思路

[[网格图]]dfs + [[递归]]：循环嵌套遍历网格图，如果不是走廊并且没统计过，进入计数。因为一旦有一块与走廊相邻整块都作废，所以设一个布尔值判断是否与走廊相邻；如果不是，则更新答案，否则不更新答案。进入 dfs 递归，如果与走廊相邻或不合法，直接返回 0，更新布尔值；否则标记当前地块，进一步递归

## 代码实现

```cpp
class Solution {
    static constexpr int DIRS[4][2] = {
        {0, -1}, {0, 1}, {-1, 0}, {1, 0}}; // 左右上下

private：
    int dfs(vector<string>& grid, int i, int j, char cur,
            bool& touch_corridor) {
        int row = grid.size();
        int column = grid[0].size();
        // 判断是否与走廊相接
        if (i < 0 || i >= row || j < 0 || j >= column || grid[i][j] == '0') {
            touch_corridor = true;
            return 0;
        }
        // 判断是否为有效主题
        if (grid[i][j] != cur || grid[i][j] == '6') {
            return 0;
        }
        grid[i][j] = '6';  // 标记为已统计
        int cnt = 1;
        for (auto [dx, dy] : DIRS) {
            int x = i + dx, y = j + dy;
            cnt += dfs(grid, x, y, cur, touch_corridor);
        }
        return cnt;
    };

public:
    int largestArea(vector<string>& grid) {
        int ans = 0;
        int row = grid.size();
        int column = grid[0].size();

        for (int i = 0; i < row; i++) {
            for (int j = 0; j < column; j++) {
                if (grid[i][j] != '0' && grid[i][j] != '6') {
                    bool touch_corridor = false;
                    int area = dfs(grid, i, j, grid[i][j], touch_corridor);
                    if (!touch_corridor) {
                        ans = max(ans, area);
                    }
                }
            }
        }
        return ans;
    }
};
```

- lambda函数版本：

```cpp
class Solution {
    static constexpr int DIRS[4][2] = {
        {0, -1}, {0, 1}, {-1, 0}, {1, 0}}; // 左右上下

public:
    int largestArea(vector<string>& grid) {
        int ans = 0;
        int row = grid.size();
        int column = grid[0].size();

        for (int i = 0; i < row; i++) {
            for (int j = 0; j < column; j++) {
                if (grid[i][j] != '0' && grid[i][j] != '6') {
                    bool touch_corridor = false;
                    char cur = grid[i][j];
                    auto dfs = [&](this auto&& dfs, int i, int j) -> int {
                        // 判断是否与走廊相接
                        if (i < 0 || i >= row || j < 0 || j >= column ||
                            grid[i][j] == '0') {
                            touch_corridor = true;
                            return 0;
                        }
                        // 判断是否为有效主题
                        if (grid[i][j] != cur || grid[i][j] == '6') {
                            return 0;
                        }
                        grid[i][j] = '6';
                        int cnt = 1;
                        for (auto [dx, dy] : DIRS) {
                            int x = i + dx, y = j + dy;
                            cnt += dfs(x, y);
                        }
                        return cnt;
                    };
                    int area = dfs(i, j);
                    if (!touch_corridor) {
                        ans = max(ans, area);
                    }
                }
            }
        }
        return ans;
    }
};
```
## 复杂度分析

### 时间复杂度：**O(mn)**

### 空间复杂度：**O(mn)**

## 总结 / 错误反思

## 知识补充

