---
title: '[Algorithms] LeetCode 206. 反转链表'
date: 2025-04-08 18:40:01
tags:
categories: 
- Algorithms
- LinkedList
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)

## 双指针

这题又忘记怎么做了。只需要`cur` `pre`两个指针，一开始各自指向`head` `nullptr`.

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
    ListNode* reverseList(ListNode* head) {
        ListNode *pre = nullptr;
        ListNode *cur = head;
        while (cur != nullptr) {
            ListNode *temp = pre;
            pre = cur;
            cur = cur->next;
            pre->next = temp;
        }
        return pre;
    }
};
```

---

## 递归

其实和双指针很像。

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
    ListNode *reverse(ListNode *cur, ListNode *pre) {
        if (cur == nullptr) {
            return pre;
        }
        ListNode *temp = pre;
        pre = cur;
        cur = cur->next;
        pre->next = temp;
        return reverse(cur, pre);
    }
    ListNode* reverseList(ListNode* head) {
        return reverse(head, nullptr);
    }
};
```

---

## 从后往前翻转指针指向

### 💡 举个例子：链表反转中的递归细节

```rust
1 -> 2 -> 3 -> 4 -> 5 -> NULL
```

递归处理后，后面的部分已经被反转成了：

```rust
5 -> 4 -> 3 -> 2
```

现在我们要做的，就是把 `1` 接到 `2` 的后面。

你可以这样理解：

```cpp
head = 1;             // 当前处理的节点是 1
head->next = 2;       // 下一个节点是 2

// 关键操作：让 2 的 next 指向 1
head->next->next = head;   // 即 2->next = 1，实现反转

// 为了避免死循环，把 1 的 next 设为 NULL
head->next = NULL;
```

最终结果：

```rust
5 -> 4 -> 3 -> 2 -> 1 -> NULL
```

四刷：要先保存反转后的链表，这样的话当前节点的下一个节点实际上就是反转后的末尾节点，那么末尾节点的下一个节点就是当前节点，当前节点现在成为了末尾节点，下一个节点就应该是空指针。最后返回黑箱子就可以了。

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
    ListNode* reverseList(ListNode* head) {
        if (head == nullptr) {
            return nullptr; // 按理来说有下面那条判断是不会出现head为空的，只是为了防止头节点本身就是空的情况
        }

        if (head->next == nullptr) {
            return head;
        }

        ListNode *node = reverseList(head->next); // 这就是一个黑箱子，返回的是一个反转了的链表
        head->next->next = head;
        head->next = nullptr;
        return node;
    }
};
```

---
