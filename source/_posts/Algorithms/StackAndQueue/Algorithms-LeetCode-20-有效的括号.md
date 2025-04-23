---
title: '[Algorithms] LeetCode 20. 有效的括号'
date: 2025-04-21 16:19:13
tags:
categories:
- Algorithms
- StackAndQueue
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/)

## 栈模拟

注意最后判断空栈。

```cpp
class Solution {
public:
    bool isValid(string s) {
        stack<char> st;
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == '(' || s[i] == '{' || s[i] == '[') {
                st.push(s[i]);
                continue;
            } 

            if (st.empty()) {
                return false;
            }

            if (s[i] == '}') {
                if (st.top() == '{') {
                    st.pop();
                } else {
                    return false;
                }
            }

            if (s[i] == ']') {
                if (st.top() == '[') {
                    st.pop();
                } else {
                    return false;
                }
            }

            if (s[i] == ')') {
                if (st.top() == '(') {
                    st.pop();
                } else {
                    return false;
                }
            }
        }

        if (!st.empty()) {
            return false;
        }
        return true;
    }
};
```

---
