---
title: '[Algorithms] LeetCode 59. 螺旋矩阵 II'
date: 2025-04-07 19:11:59
tags:
categories: 
- Algorithms
- Array
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 59. 螺旋矩阵 II](https://leetcode.cn/problems/spiral-matrix-ii)

考察基本功啊，区间统一**左闭右开**，最后记得处理n为奇数的情况。

```cpp
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> v(n, vector<int>(n));

        int index = 1;
        int startIndex = 0;
        while (startIndex <= n / 2) {
            for (int j = startIndex; j < n - startIndex - 1; j++) {
                v[startIndex][j] = index;
                index++;
            }
            for (int i = startIndex; i < n - startIndex - 1; i++) {
                v[i][n - startIndex - 1] = index;
                index++;
            }
            for (int j = n - startIndex - 1; j > startIndex; j--) {
                v[n - startIndex - 1][j] = index;
                index++;
            }
            for (int i = n - startIndex - 1; i > startIndex; i--) {
                v[i][startIndex] = index;
                index++;
            }
            startIndex++;
        }
        if (n % 2) {
            v[n / 2][n / 2] = n * n;
        }
        return v;
    }
};
```

---
