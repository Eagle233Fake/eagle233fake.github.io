---
title: '[Algorithms] LeetCode 28. 找出字符串中第一个匹配项的下标'
date: 2025-04-13 11:25:09
tags:
categories:
- Algorithms
- String
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 28. 找出字符串中第一个匹配项的下标](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)

## KMP

KMP实际上是优化过的滑动窗口，只不过优化得有点太巧妙了。

看这个视频：[Knuth–Morris–Pratt(KMP) Pattern Matching(Substring search)](https://www.youtube.com/watch?v=GTJr8OvyEVQ)

理解了写代码就好，不要背模板。

`next[j] = i + 1`，长度是下标加1，不是`next[j] = next[j - 1] + 1`！！！

```cpp
class Solution {
public:
    void kmp(vector<int> &next, string s) {
        int i = 0, j = 1;
        while (j < next.size()) {
            if (s[i] == s[j]) {
                next[j] = i + 1; // 长度是下标加1
                j++;
                i++;
                continue;
            } else {
                if (i == 0) {
                    j++;
                    continue;
                }
                i = next[i - 1];
                continue;
            }
        }
    }

    int strStr(string haystack, string needle) {
        vector<int> next(needle.size(), 0);
        kmp(next, needle);

        int i = 0, j = 0;
        while (j < haystack.size()) {
            if (haystack[j] == needle[i]) {
                i++;
                j++;
            } else {
                if (i == 0) {
                    j++;
                    continue;
                }
                i = next[i - 1];
            }

            if (i == needle.size()) {
                return j - i; // 返回首位的index
            }
        }

        return -1;
    }
};
```

---

## 滑动窗口 双指针

注意回撤操作，`j`如果不匹配要回到首位`index + 1`。

```cpp
class Solution {
public:
    int strStr(string haystack, string needle) {
        int i = 0, j = 0;
        while (j < haystack.size()) {
            if (haystack[j] == needle[i]) {
                j++;
                i++;
            } else {
                if (i == 0) {
                    j++;
                    continue;
                }
                j = j - i + 1; // 刚开始首位的index加上1
                i = 0;
            }

            if (i == needle.size()) {
                return j - i;
            }
        }

        return -1;
    }
};
```

---
