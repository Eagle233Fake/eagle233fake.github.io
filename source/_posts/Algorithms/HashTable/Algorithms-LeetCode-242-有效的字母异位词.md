---
title: '[Algorithms] LeetCode 242. 有效的字母异位词'
date: 2025-04-09 16:30:18
tags:
categories: 
- Algorithms
- HashTable
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 242. 有效的字母异位词](https://leetcode.cn/problems/valid-anagram/)

## 数组作为哈希表

```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        vector<int> a(26, 0);
        for (int i = 0; i < s.size(); i++) {
            a[s[i] - 'a']++;
        }

        vector<int> b(26, 0);
        for (int i = 0; i < t.size(); i++) {
            b[t[i] - 'a']++;
        }

        if (a == b) {
            return true;
        }
        return false;
    }
};
```

也可以不使用`vector`的`==`。

```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        vector<int> a(26, 0);
        if (s.size() != t.size()) {
            return false;
        }
        for (int i = 0; i < s.size(); i++) {
            a[s[i] - 'a']++;
            a[t[i] - 'a']--;
        }

        for (int i = 0; i < 26; i++) {
            if (a[i] != 0) {
                return false;
            }
        }

        return true;
    }
};
```

---
