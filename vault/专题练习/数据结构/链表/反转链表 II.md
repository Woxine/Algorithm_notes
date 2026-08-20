---
date: 2025-09-21
tags:
  - 链表
  - 中等
---
[反转链表 II](https://leetcode.cn/problems/reverse-linked-list-ii/)
## 解题思路

[[链表]]：

```cpp
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int left, int right) {
        ListNode dummy(0, head);
        ListNode* pre0 = &dummy;
        // 定位到要反转的部分的前一个节点
        for (int i = 0; i < left - 1; i++) {
            pre0 = pre0->next;
        }
		
        ListNode* pre = nullptr;
        ListNode* curr = pre0->next;
        // 开始反转
        for (int j = 0; j < right - left + 1; j++) {
            ListNode* nxt = curr->next;
            curr->next = pre;
            pre = curr;
            curr = nxt;
        }
		
        pre0->next->next = curr;
        pre0->next = pre;

        return dummy.next;
    }
};
```

## 复杂度分析

### 时间复杂度：**O(n)**

### 空间复杂度：**O(1)**

## 新知识点

## 总结

