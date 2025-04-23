---
title: '[Algorithms] LeetCode 24. 两两交换链表中的节点'
date: 2025-04-08 19:35:03
tags:
categories:
- Algorithms
- LinkedList
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 24. 两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/)

## 三指针

三指针，其中当前指针只要定义一个，其他两个在while循环里面定义就好，这样在判断时只有一个变量，也只要改变一次cur的值。

画图是关键，不要懒！

三刷：一定要定义虚拟头节点

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode *dummyHead = new ListNode(0, head);
        ListNode *cur = dummyHead;

        while (cur->next != nullptr && cur->next->next != nullptr) {
            ListNode *temp1 = cur->next;
            ListNode *temp2 = temp1->next;

            cur->next = temp2;
            temp1->next = temp2->next;
            temp2->next = temp1;

            cur = temp1;
        }

        head = dummyHead->next;
        delete dummyHead;
        return head;
    }
};
```

---
