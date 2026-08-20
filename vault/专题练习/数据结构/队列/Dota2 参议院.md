---
date: 2025-09-10
tags:
  - 队列
  - 贪心算法
  - 字符串
---
[Dota2 参议院](https://leetcode.cn/problems/dota2-senate/)
## 解题思路

- 单[[队列]]：我最开始的思路，将整个字符串都当成[[队列]]，然后不断地将队首的元素移到队尾（进入下一轮），同时记录“需要淘汰的对方议员数”，如果这个数不为 0 ，则相应的议员淘汰；最后统计双方阵营剩余人员，一方为 0 则跳出循环

```cpp
class Solution {
public:
    string predictPartyVictory(string senate) {
        queue<char> q;
        int banR = 0, banD = 0; // 待淘汰的R和D数量
        // 统计初始阵营数量
        int cntR = count(senate.begin(), senate.end(), 'R');
        int cntD = senate.size() - cntR;
        // 初始化队列
        for (char c : senate) q.push(c);
        
        while (cntR > 0 && cntD > 0) {
            char c = q.front();
            q.pop();
            
            if (c == 'R') {
                if (banR > 0) {
                    // 当前R被淘汰
                    banR--;
                    cntR--;
                } else {
                    // 禁止一个D，当前R进入下一轮
                    banD++;
                    q.push(c);
                }
            } else {
                if (banD > 0) {
                    // 当前D被淘汰
                    banD--;
                    cntD--;
                } else {
                    // 禁止一个R，当前D进入下一轮
                    banR++;
                    q.push(c);
                }
            }
        }
        
        return cntR > 0 ? "Radiant" : "Dire";
    }
};
```

- 双队列：创建两个队列分别存储双方议员的下标（先后顺序），然后贪心地淘汰最近的对方议员，并进入下一轮（[[贪心算法]]）

```cpp
class Solution {
public:
    string predictPartyVictory(string senate) {
        queue<int> rad, dir;
        int n = senate.size();
        // 初始化队列
        for (int i = 0; i < n; i++) {
            if (senate[i] == 'R') {
                rad.push(i);
            } else {
                dir.push(i);
            }
        }
        while (!rad.empty() && !dir.empty()) {
	        // 优先获得发言权的一方贪心地淘汰一名最近的对方议员
            if (rad.front() < dir.front()) {
	            // 进入下一轮
                rad.push(rad.front() + n);
            } else {
                dir.push(dir.front() + n);
            }
            rad.pop();
            dir.pop();
        }
        return rad.empty() ? "Dire" : "Radiant";
    }
};
```

三目运算符简化：

```cpp
class Solution {
public:
    string predictPartyVictory(string senate) {
        queue<int> rad, dir;
        int n = senate.size();
        for (int i = 0; i < n; i++) {
            (senate[i] == 'R' ? rad : dir).push(i);
        }
        while (!rad.empty() && !dir.empty()) {
            int r = rad.front(), d = dir.front();
            (r < d ? rad : dir).push(r < d ? r + n : d + n);
            rad.pop(), dir.pop();
        }
        return rad.empty() ? "Dire" : "Radiant";
    }
};
```
## 复杂度分析

### 时间复杂度：**O(n)**

### 空间复杂度：**O(n)**

## 新知识点

## 总结

