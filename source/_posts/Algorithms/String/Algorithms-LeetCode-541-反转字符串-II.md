---
title: '[Algorithms] LeetCode 541. 反转字符串 II'
date: 2025-04-13 11:11:16
tags:
categories:
- Algorithms
- String
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 541. 反转字符串 II](https://leetcode.cn/problems/reverse-string-ii/)

## 单指针

玩几天脑子进水了，这题都不会做了。后继比较难的可以直接用`reverse`，没必要再自己写了。

```cpp
class Solution {
public:
    string reverseStr(string s, int k) {
        for (int i = 0; i < s.size(); i += 2 * k) {
            if (i + k < s.size()) {
                reverse(s.begin() + i, s.begin() + i + k);
            } else {
                reverse(s.begin() + i, s.end());
            }
        }

        return s;
    }
};
```

---
