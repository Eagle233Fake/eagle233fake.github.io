---
title: '[Algorithms] KamaCoder 58. 区间和（第九期模拟笔试）'
date: 2025-04-07 19:22:11
tags:
categories: 
- Algorithms
- Array
---

> 来源：[代码随想录](https://programmercarl.com/)

[KamaCoder 58. 区间和（第九期模拟笔试）](https://kamacoder.com/problempage.php?pid=1070)

前缀和的思想。

```cpp
#include <bits/stdc++.h>

using namespace std;

int main(void) {
    int n;
    cin >> n;
    vector<int> v(n);
    for (int i = 0; i < n; i++) {
        cin >> v[i];
    }

    for (int i = 1; i < n; i++) {
        v[i] += v[i - 1];
    }

    int a, b;
    while (cin >> a >> b) {
        if (a == 0) {
            cout << v[b] << endl;
        } else {
            cout << v[b] - v[a - 1] << endl; 
        }
    }
}
```

---
