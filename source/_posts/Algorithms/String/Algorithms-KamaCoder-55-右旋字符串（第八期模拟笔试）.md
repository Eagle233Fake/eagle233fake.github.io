---
title: '[Algorithms] KamaCoder 55. 右旋字符串（第八期模拟笔试）'
date: 2025-04-13 11:25:00
tags:
categories:
- Algorithms
- String
---

> 来源：[代码随想录](https://programmercarl.com/)

[KamaCoder 55. 右旋字符串（第八期模拟笔试）](https://kamacoder.com/problempage.php?pid=1065)

## 三次反转

```cpp
#include <bits/stdc++.h>

using namespace std;

int main(void) {
    int k;
    string s;
    cin >> k >> s;

    int index = s.size() - k;
    reverse(s.begin(), s.begin() + index);
    reverse(s.begin() + index, s.end());

    reverse(s.begin(), s.end());

    cout << s << endl;
}
```

---
