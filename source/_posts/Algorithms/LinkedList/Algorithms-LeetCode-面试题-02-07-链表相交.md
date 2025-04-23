---
title: '[Algorithms] LeetCode 面试题 02.07. 链表相交'
date: 2025-04-08 19:52:07
tags:
categories:
- Algorithms
- LinkedList
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 面试题 02.07. 链表相交](https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/)

## 求长度之差

比谁长，谁长谁先走，最后一起走，走到一样的就返回。

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        int lenA = 0;
        int lenB = 0;
        ListNode *p1 = headA;
        while (p1 != nullptr) {
            p1 = p1->next;
            lenA++;
        }
        p1 = headB;
        while (p1 != nullptr) {
            p1 = p1->next;
            lenB++;
        }

        bool isALonger = false;
        if (lenA > lenB) {
            isALonger = true;
        }

        int len = abs(lenA - lenB);

        p1 = headA;
        ListNode *p2 = headB;

        if (isALonger) {
            while (len--) {
                p1 = p1->next;
            }
        } else {
            while (len--) {
                p2 = p2->next;
            }
        }

        while (p1 != nullptr && p2 != nullptr && p1 != p2) {
            p1 = p1->next;
            p2 = p2->next;
        }

        return p1;
    }
};
```

---
