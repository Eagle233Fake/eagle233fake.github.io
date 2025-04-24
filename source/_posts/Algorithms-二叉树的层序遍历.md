---
title: '[Algorithms] 二叉树的层序遍历'
date: 2025-04-24 20:06:46
tags:
categories: 
- Algorithms
- BinaryTree
---

> 来源：[代码随想录](https://programmercarl.com/)

## 二叉树的层序遍历

[LeetCode 102. 二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/)

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        queue<TreeNode *> que;
        vector<int> v;
        vector<vector<int>> vv;
        if (root != nullptr) {
            que.push(root);
        }

        while (!que.empty()) {
            int size = que.size();
            for (int i = 0; i < size; i++) {
                TreeNode *node = que.front();
                que.pop();
                v.push_back(node->val);

                if (node->left) {
                    que.push(node->left);
                }
                if (node->right) {
                    que.push(node->right);
                }
            }
            vv.push_back(v);
            v.clear();
        }

        return vv;
    }
};
```

---

## 二叉树的层次遍历 II

[LeetCode 107. 二叉树的层次遍历 II](https://leetcode.cn/problems/binary-tree-level-order-traversal-ii/)

```cpp

```

---

## 二叉树的右视图

[LeetCode 199. 二叉树的右视图](https://leetcode.cn/problems/binary-tree-right-side-view/)

```cpp

```

---

## 二叉树的层平均值

[LeetCode 637. 二叉树的层平均值](https://leetcode.cn/problems/average-of-levels-in-binary-tree/)

```cpp

```

---

## N 叉树的层序遍历

[LeetCode 429. N 叉树的层序遍历](https://leetcode.cn/problems/n-ary-tree-level-order-traversal/)

```cpp

```

---

## 在每个树行中找最大值

[LeetCode 515. 在每个树行中找最大值](https://leetcode.cn/problems/find-largest-value-in-each-tree-row/)

```cpp

```

---

## 填充每个节点的下一个右侧节点指针

[LeetCode 116. 填充每个节点的下一个右侧节点指针](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node/)

```cpp

```

---

## 填充每个节点的下一个右侧节点指针 II

[LeetCode 117. 填充每个节点的下一个右侧节点指针 II](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node-ii/)

```cpp

```

---

## 二叉树的最大深度

[LeetCode 104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

```cpp

```

---

## 二叉树的最小深度

[LeetCode 111. 二叉树的最小深度](https://leetcode.cn/problems/minimum-depth-of-binary-tree/)

```cpp

```

---
