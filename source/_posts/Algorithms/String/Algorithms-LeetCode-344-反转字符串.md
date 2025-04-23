---
title: '[Algorithms] LeetCode 344. 反转字符串'
date: 2025-04-13 10:52:58
tags:
categories:
- Algorithms
- String
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 344. 反转字符串](https://leetcode.cn/problems/reverse-string/)

## 单指针

```cpp
class Solution {
public:
    void reverseString(vector<char>& s) {
        for (int i = 0; i < s.size() / 2; i++) {
            char temp = s[i];
            s[i] = s[s.size() - 1 - i];
            s[s.size() - 1 - i] = temp;
        }
    }
};
```

也可以用`swap`函数。

---
