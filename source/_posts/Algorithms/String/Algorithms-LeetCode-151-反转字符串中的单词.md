---
title: '[Algorithms] LeetCode 151. 反转字符串中的单词'
date: 2025-04-13 11:24:33
tags:
categories:
- Algorithms
- String
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 151. 反转字符串中的单词](https://leetcode.cn/problems/reverse-words-in-a-string/)

## `stringstream`

利用`stringstream`可以快速分割单词。每个单词反转后再全部反转。

```cpp
class Solution {
public:
    string reverseWords(string s) {
        stringstream ss(s);
        string word;
        string s2;
        while (ss >> word) {
            reverse(word.begin(), word.end());
            s2 += word;
            s2 += " ";
        }
        s2.resize(s2.size() - 1);

        reverse(s2.begin(), s2.end());
        return s2;
    }
};
```

---

## 双指针

处理前、中、后的空格是关键。注意`while`循环里面也要写越界判断。

```cpp
class Solution {
public:
    string reverseWords(string s) {
        int slow = 0;
        int fast = 0;
        bool isStart = true;
        while (fast < s.size()) {
            while (isStart && s[fast] == ' ') {
                fast++;
            }
            if (s[fast] != ' ') {
                isStart = false;
                int index = slow;
                while (s[fast] != ' ' && fast < s.size()) {
                    s[slow] = s[fast];
                    fast++;
                    slow++;
                }
                reverse(s.begin() + index, s.begin() + slow);
            }
            if (!isStart && s[fast] == ' ') {
                s[slow] = ' ';
                slow++;
                fast++;
                while (s[fast] == ' ' && fast < s.size()) {
                    fast++;
                }
            }
        }
        s.resize(slow);

        for (int i = s.size() - 1; i >= 0; i--) {
            if (s[i] == ' ') {
                s.resize(s.size() - 1);
            } else {
                break;
            }
        }
        reverse(s.begin(), s.end());
        return s;
    }
};
```

---
