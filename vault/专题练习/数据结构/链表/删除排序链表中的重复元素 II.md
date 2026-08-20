---
date: 2025-09-19
tags:
  - 链表
  - 简单
---
[删除排序链表中的重复元素 II](https://leetcode.cn/problems/remove-duplicates-from-sorted-list-ii/)
## 解题思路

[[链表]]：这次重复的元素需要全部删除，而不是只留一个了，所以我们需要判断：哪一个值出现了一次以上？然后查找每一个有相同值的节点，把它删除

```cpp
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        // 创建哨兵节点，因为可能从头节点开始就有重复值
        ListNode dummy(0, head);
        auto curr = &dummy;
        // 考察下一个节点和下下个节点
        while (curr->next && curr->next->next) {
            int val = curr->next->val;
            // 值一样就开始删除
            if (curr->next->next->val == val) {
	            // 直到没东西可删或出现不一样的值
                while (curr->next && curr->next->val == val) {
                    curr->next = curr->next->next;
                }
            } else {
                curr = curr->next;
            }
        }
        return dummy.next;
    }
};
```

## 复杂度分析

### 时间复杂度：**O(n)**

尽管 while 中嵌套了一个 while ，但是每次操作都会删除一个节点或移动一个位置，所以最终的复杂度还是 O(n) ，其中 n 是链表长度

### 空间复杂度：**O(1)**

## 新知识点

## 总结

因为是排好序的，所以其实不需要额外的容器来去重