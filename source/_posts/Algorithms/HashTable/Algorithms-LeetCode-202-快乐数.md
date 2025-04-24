---
title: '[Algorithms] LeetCode 202. 快乐数'
date: 2025-04-09 16:39:56
tags:
categories: 
- Algorithms
- HashTable
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 202. 快乐数](https://leetcode.cn/problems/happy-number/)

## 用`unordered_set`保存出现过的结果

四刷：先插入再更新。

```cpp
class Solution {
public:
    int getSum(int n) {
        int sum = 0;
        while (n) {
            sum += (n % 10) * (n % 10);
            n /= 10;
        }
        return sum;
    }

    bool isHappy(int n) {
        unordered_set<int> us;
        while (us.find(n) == us.end() && n != 1) {
            us.insert(n);
            n = getSum(n);
        }

        if (n == 1) {
            return true;
        }
        return false;
    }
};
```

---
