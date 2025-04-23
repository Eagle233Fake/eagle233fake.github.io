---
title: '[Algorithms] KamaCoder 44. 开发商购买土地（第五期模拟笔试）'
date: 2025-04-07 19:26:08
tags:
categories: 
- Algorithms
- Array
---

> 来源：[代码随想录](https://programmercarl.com/)

[KamaCoder 44. 开发商购买土地（第五期模拟笔试）](https://kamacoder.com/problempage.php?pid=1044)

前缀和Plus。有两种划分，那么分别开两个数组，把二维数组从行列两个方向压缩成列行和，然后再用前缀和。观察到求划分只要让i从0遍历到n-2，再用**n-1的元素各自减去第i个元素**，此时减得的大小和第i个元素就是两种划分的各自大小，求差值绝对值再取最小值即可。

三刷发现没有仔细揣摩差值怎么求，还要注意一下。

```cpp
#include <bits/stdc++.h>

using namespace std;

int main(void) {
    int n, m;
    cin >> n >> m;

    vector<vector<int>> v(n, vector<int>(m));

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> v[i][j];
        }
    }

    vector<int> row(n, 0);
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            row[i] += v[i][j];
        }
    }

    vector<int> col(m, 0);
    for (int j = 0; j < m; j++) {
        for (int i = 0; i < n; i++) {
            col[j] += v[i][j];
        }
    }

    for (int i = 1; i < n; i++) {
        row[i] += row[i - 1];
    }

    for (int j = 1; j < m; j++) {
        col[j] += col[j - 1];
    }

    int result = INT_MAX;

    for (int i = 0; i < n - 1; i++) {
        result = min(result, abs(row[n - 1] - 2 * row[i]));
    }

    for (int j = 0; j < m - 1; j++) {
        result = min(result, abs(col[m - 1] - 2 * col[j]));
    }

    cout << result << endl;
}
```

---
