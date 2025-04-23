---
title: '[Algorithms] LeetCode 1047. 删除字符串中的所有相邻重复项'
date: 2025-04-21 16:26:38
tags:
categories:
- Algorithms
- StackAndQueue
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 1047. 删除字符串中的所有相邻重复项](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/)

## 栈模拟

注意是两两删除。

```cpp
class Solution {
public:
    string removeDuplicates(string s) {
        stack<char> st;
        for (int i = 0; i < s.size(); i++) {
            if (st.empty() || st.top() != s[i]) {
                st.push(s[i]);
            } else {
                st.pop();
            }
        }

        string l;
        while (!st.empty()) {
            l += st.top();
            st.pop();
        }

        reverse(l.begin(), l.end());
        return l;
    }
};
```

---
