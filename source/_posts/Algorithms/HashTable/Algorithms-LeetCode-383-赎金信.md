---
title: '[Algorithms] LeetCode 383. 赎金信'
date: 2025-04-10 13:27:34
tags:
categories: 
- Algorithms
- HashTable
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 383. 赎金信](https://leetcode.cn/problems/ransom-note/)

## `unordered_map`

```cpp
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        unordered_map<int, int> umap;
        for (int i = 0; i < magazine.size(); i++) {
            umap[magazine[i]]++;
        }
        for (int i = 0; i < ransomNote.size(); i++) {
            umap[ransomNote[i]]--;
        }

        for (auto iter : umap) {
            if (iter.second < 0) {
                return false;
            }
        }

        return true;
    }
};
```

---

## 数组

```cpp
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        vector<int> a(26, 0);

        for (int i = 0; i < magazine.size(); i++) {
            a[magazine[i] - 'a']++;
        }

        for (int i = 0; i < ransomNote.size(); i++) {
            a[ransomNote[i] - 'a']--;
        }

        for (int i : a) {
            if (i < 0) {
                return false;
            }
        }

        return true;
    }
};
```

---
