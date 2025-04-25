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

两次反转。

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
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        vector<vector<int>> vv;
        queue<TreeNode *> que;
        if (root != nullptr) {
            que.push(root);
        }

        while (!que.empty()) {
            int size = que.size();
            vector<int> v;
            for (int i = 0; i < size; i++) {
                TreeNode *node = que.front();
                que.pop();
                v.push_back(node->val);
                if (node->right) {
                    que.push(node->right);
                }
                if (node->left) {
                    que.push(node->left);
                }
            }
            reverse(v.begin(), v.end());
            vv.push_back(v);
        }

        reverse(vv.begin(), vv.end());
        return vv;
    }
};
```

---

## 二叉树的右视图

[LeetCode 199. 二叉树的右视图](https://leetcode.cn/problems/binary-tree-right-side-view/)

单独处理`size - 1`。

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
    vector<int> rightSideView(TreeNode* root) {
        queue<TreeNode *> que;
        vector<int> v;
        if (root != nullptr) {
            que.push(root);
        }

        while (!que.empty()) {
            int size = que.size();
            for (int i = 0; i < size; i++) {
                TreeNode *node = que.front();
                que.pop();
                if (node->left) {
                    que.push(node->left);
                }
                if (node->right) {
                    que.push(node->right);
                }

                if (i == size - 1) {
                    v.push_back(node->val);
                }
            } 
        }

        return v;
    }
};
```

---

## 二叉树的层平均值

[LeetCode 637. 二叉树的层平均值](https://leetcode.cn/problems/average-of-levels-in-binary-tree/)

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
    vector<double> averageOfLevels(TreeNode* root) {
        queue<TreeNode *> que;
        vector<double> v;
        if (root) {
            que.push(root);
        }

        while (!que.empty()) {
            int size = que.size();
            double sum = 0;
            for (int i = 0; i < size; i++) {
                TreeNode *node = que.front();
                que.pop();
                sum += node->val;
                if (node->left) {
                    que.push(node->left);
                }

                if (node->right) {
                    que.push(node->right);
                }
            }
            v.push_back(sum / size);
        }

        return v;
    }
};
```

---

## N 叉树的层序遍历

[LeetCode 429. N 叉树的层序遍历](https://leetcode.cn/problems/n-ary-tree-level-order-traversal/)

`auto iter : node->children`

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
public:
    vector<vector<int>> levelOrder(Node* root) {
        queue<Node *> que;
        vector<vector<int>> vv;
        if (root) {
            que.push(root);
        }
        while (!que.empty()) {
            int size = que.size();
            vector<int> v;
            for (int i = 0; i < size; i++) {
                Node *node = que.front();
                que.pop();
                v.push_back(node->val);
                for (auto iter : node->children) {
                    if (iter) {
                        que.push(iter);
                    }
                }
            }

            vv.push_back(v);
        }

        return vv;
    }
};
```

---

## 在每个树行中找最大值

[LeetCode 515. 在每个树行中找最大值](https://leetcode.cn/problems/find-largest-value-in-each-tree-row/)

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
    vector<int> largestValues(TreeNode* root) {
        queue<TreeNode *> que;
        vector<int> v;
        if (root) {
            que.push(root);
        }

        while (!que.empty()) {
            int size = que.size();
            int mmax = INT_MIN;
            for (int i = 0; i < size; i++) {
                TreeNode *node = que.front();
                mmax = max(mmax, node->val);
                que.pop();

                if (node->left) {
                    que.push(node->left);
                }
                if (node->right) {
                    que.push(node->right);
                }
            }

            v.push_back(mmax);
        }

        return v;
    }
};
```

---

## 填充每个节点的下一个右侧节点指针

[LeetCode 116. 填充每个节点的下一个右侧节点指针](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node/)

```cpp
Node *nodeNext = nullptr;
if (!que.empty()) {
    nodeNext = que.front();
}
node->next = nodeNext;
```

如上的方式会出现问题，每一层最后一个节点的下一个节点被指向了下一层的第一个节点。所以采用标记前一个节点的方式。

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;
    Node* next;

    Node() : val(0), left(NULL), right(NULL), next(NULL) {}

    Node(int _val) : val(_val), left(NULL), right(NULL), next(NULL) {}

    Node(int _val, Node* _left, Node* _right, Node* _next)
        : val(_val), left(_left), right(_right), next(_next) {}
};
*/

class Solution {
public:
    Node* connect(Node* root) {
        queue<Node *> que;
        if (root) {
            que.push(root);
        }

        while (!que.empty()) {
            int size = que.size();
            Node *nodePrev = nullptr;
            for (int i = 0; i < size; i++) {
                Node *node = que.front();
                if (nodePrev) {
                    nodePrev->next = node;
                }
                que.pop();
                nodePrev = node;
                if (node->left) {
                    que.push(node->left);
                }
                if (node->right) {
                    que.push(node->right);
                }
            }
        }

        return root;
    }
};
```

---

## 填充每个节点的下一个右侧节点指针 II

[LeetCode 117. 填充每个节点的下一个右侧节点指针 II](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node-ii/)

和上一道一模一样。

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;
    Node* next;

    Node() : val(0), left(NULL), right(NULL), next(NULL) {}

    Node(int _val) : val(_val), left(NULL), right(NULL), next(NULL) {}

    Node(int _val, Node* _left, Node* _right, Node* _next)
        : val(_val), left(_left), right(_right), next(_next) {}
};
*/

class Solution {
public:
    Node* connect(Node* root) {
        queue<Node *> que;
        if (root) {
            que.push(root);
        }

        while (!que.empty()) {
            Node *nodePrev = nullptr;
            int size = que.size();
            while (size--) {
                Node *node = que.front();
                if (nodePrev) {
                    nodePrev->next = node;
                }
                que.pop();
                nodePrev = node;
                if (node->left) {
                    que.push(node->left);
                }
                if (node->right) {
                    que.push(node->right);
                }
            }
        }

        return root;
    }
};
```

---

## 二叉树的最大深度

[LeetCode 104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

注意深度和高度的区别，最大深度指的是从根节点到最远叶子节点的最长路径上的节点数。

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
    int maxDepth(TreeNode* root) {
        queue<TreeNode *> que;
        if (root) {
            que.push(root);
        }

        int depth = 0;
        while (!que.empty()) {
            int size = que.size();
            depth++;
            while (size--) {
                TreeNode *node = que.front();
                que.pop();
                if (node->left) {
                    que.push(node->left);
                }
                if (node->right) {
                    que.push(node->right);
                }
            }
        }

        return depth;
    }
};
```

---

## 二叉树的最小深度

[LeetCode 111. 二叉树的最小深度](https://leetcode.cn/problems/minimum-depth-of-binary-tree/)

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
    int minDepth(TreeNode* root) {
        queue<TreeNode *> que;
        if (root) {
            que.push(root);
        }

        int depth = 0;
        while (!que.empty()) {
            int size = que.size();
            depth++;
            while (size--) {
                TreeNode *node = que.front();
                que.pop();
                if (!node->right && !node->left) {
                    return depth;
                }

                if (node->left) {
                    que.push(node->left);
                }
                if (node->right) {
                    que.push(node->right);
                }
            }
        }

        return depth;
    }
};
```

---
