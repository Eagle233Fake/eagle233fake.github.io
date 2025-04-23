---
title: '[Algorithms] LeetCode 203. 移除链表元素'
date: 2025-04-08 13:16:54
tags:
categories: 
- Algorithms
- LinkedList
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 203. 移除链表元素](https://leetcode.cn/problems/remove-linked-list-elements/)

## 虚拟头节点 Dummy Head

这题没一遍过，因为并不是每次循环都要把指针指向下一个节点，如果进行了删除操作，那么下一个节点还需要再判断，不急着向后遍历。

第三次做，删删改改才通过。为什么要双指针呢？一个指针足够了，只需保证下一个结点的值等于`val`就好了，没必要维护两个指针。结果发现自己写出的代码还是有问题，原因是嵌套两层`while`循环了。

记住如下的代码格式：如果等于就操作，不等于就下一个节点。记住！！！

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
    ListNode* removeElements(ListNode* head, int val) {
        ListNode *dummyHead = new ListNode(INT_MIN, head);
        ListNode *p = dummyHead;
        while (p->next != nullptr) {
            if (p->next->val == val) {
                ListNode *temp = p->next->next;
                delete p->next;
                p->next = temp;
            } else {
                p = p->next; // 并不是每次都要next
            }
        }

        head = dummyHead->next;
        delete dummyHead;
        return head;
    }
};
```

---

## 递归

好抽象，以后回来再看看。

碰到空节点就返回；碰到等于当前值的就跳过，返回下一个；否则让当前节点的下一个节点等于递归，返回当前节点。

处理我的下一个节点，再返回我想返回的情况给我上一层节点。

四刷：

```cpp
if (head->val == val) {
    ListNode *node = remove(head->next, val);
    delete head;
    return node;
}
```

注意这里的逻辑，因为要递归处理，所以不能简单返回下两层，防止有的元素没有被删除。

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
    ListNode* remove(ListNode* head, int val) {
        if (head == nullptr) {
            return head;
        }

        if (head->val == val) {
            ListNode *node = remove(head->next, val);
            delete head;
            return node;
        }
        head->next = remove(head->next, val);
        return head;
    }

    ListNode* removeElements(ListNode* head, int val) {
        return remove(head, val);
    }
};
```

---
