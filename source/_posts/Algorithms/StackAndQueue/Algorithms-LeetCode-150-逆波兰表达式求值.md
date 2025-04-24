---
title: '[Algorithms] LeetCode 150. 逆波兰表达式求值'
date: 2025-04-22 13:39:21
tags:
categories:
- Algorithms
- StackAndQueue
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 150. 逆波兰表达式求值](https://leetcode.cn/problems/evaluate-reverse-polish-notation/)

## 栈模拟

注意`string`的输入输出处理，小心负数。

```cpp
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<int> st;
        for (string s : tokens) {
            if (s == "+" || s == "-" || s == "*" || s == "/") {
                char c = s[0];
                int a = st.top();
                st.pop();
                int b = st.top();
                st.pop();
                if (c == '+') {
                    st.push(b + a);
                } else if (c == '-') {
                    st.push(b - a);
                } else if (c == '*') {
                    st.push(b * a);
                } else {
                    st.push(b / a);
                }
            } else {
                st.push(stoi(s));
            }
        }

        return st.top();
    }
};
```

---
