---
title: '[Algorithms] LeetCode 459. 重复的子字符串'
date: 2025-04-13 11:25:17
tags:
categories:
- Algorithms
- String
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 459. 重复的子字符串](https://leetcode.cn/problems/repeated-substring-pattern/)

## KMP

如果 **`len % (len - next[len - 1]) == 0`并且`next[len - 1] != 0`** ，则说明数组的长度正好可以被**最长相等前后缀不包含的子串的长度**整除 ，说明该字符串有重复的子字符串。

```cpp
class Solution {
public:
    void kmp(vector<int> &next, string &s) {
        int i = 0, j = 1;
        while (j < s.size()) {
            if (s[i] == s[j]) {
                next[j] = i + 1;
                i++;
                j++;
            } else {
                if (i == 0) {
                    j++;
                    continue;
                }
                i = next[i - 1];
            }
        }
    }

    bool repeatedSubstringPattern(string s) {
        vector<int> next(s.size(), 0);
        kmp(next, s);

        if (s.size() % (s.size() - next[s.size() - 1]) == 0 && next[s.size() - 1] != 0) { // 注意两个判断条件
            return true;
        }

        return false;
    }
};
```

---

## 字符串双拼 + 掐头去尾

暴力解法。注意`string::npos`实际上是`-1` (`1 << 31`)。

三刷：`erase`用法

```cpp
static const size_t npos = -1;
```

```cpp
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        string ss = s + s;
        ss.erase(ss.begin());
        ss.erase(ss.end() - 1);

        if (ss.find(s) != string::npos) { // 找到了
            return true;
        }
        return false;
    }
};
```

---
