---
title: '[Algorithms] LeetCode 19. 删除链表的倒数第 N 个结点'
date: 2025-04-08 19:39:36
tags:
categories:
- Algorithms
- LinkedList
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

## 双指针

虚拟头节点很管用，经常能够应付莫名其妙的空指针问题。其实也不是莫名其妙，只是能给自己少找点麻烦。就像高中的时候，明明上课都没听懂，就没必要逼着自己还要记笔记了。
快指针先走，二者刚好能隔开n-1的距离。

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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode *dummyHead = new ListNode(0, head);
        ListNode *slow = dummyHead;
        ListNode *fast = dummyHead;
        while (n--) {
            fast = fast->next;
        }

        while (fast->next != nullptr) {
            fast = fast->next;
            slow = slow->next;
        }

        fast = slow->next;
        slow->next = fast->next;
        delete fast;
        head = dummyHead->next;
        delete dummyHead;
        return head;
    }
};
```

---
