---
title: '[Algorithms] LeetCode 142. 环形链表 II'
date: 2025-04-08 20:03:11
tags:
categories:
- Algorithms
- LinkedList
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)

## 双指针

没什么好说的，写到就是赚到，没写到就偶偶偶买嘎了。

一个走1一个走2，碰到的时候同时移动头节点和其中一个节点，相遇的就是入口。

`while`里面就处理结果，这样不用考虑别的情况需要怎么判断了。

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
    ListNode *detectCycle(ListNode *head) {
        ListNode *slow = head;
        ListNode *fast = head;

        while (fast != nullptr && fast->next != nullptr) {
            slow = slow->next;
            fast = fast->next->next;
            if (slow == fast) {
                while (head != slow) {
                    head = head->next;
                    slow = slow->next;
                }
                return head;
            }
        }
        return nullptr;   
    }
};
```
