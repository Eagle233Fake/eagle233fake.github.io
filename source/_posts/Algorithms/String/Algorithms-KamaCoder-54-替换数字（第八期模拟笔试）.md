---
title: '[Algorithms] KamaCoder 54. 替换数字（第八期模拟笔试）'
date: 2025-04-13 11:18:32
tags:
categories:
- Algorithms
- String
---

> 来源：[代码随想录](https://programmercarl.com/)

[KamaCoder 54. 替换数字（第八期模拟笔试）](https://kamacoder.com/problempage.php?pid=1064)

## 双指针

每次遇到数字都把第二个指针指向五个元素后，将字符串复制到后面。

```cpp
#include <bits/stdc++.h>

using namespace std;

int main(void) {
    string s;
    cin >> s;

    for (int i = 0; i < s.size(); i++) {
        if (isdigit(s[i])) {
            s.resize(s.size() + 5);
            for (int j = s.size() - 1; j > i; j--) {
                s[j] = s[j - 5];
            }
            s[i] = 'n';
            s[i + 1] = 'u';
            s[i + 2] = 'm';
            s[i + 3] = 'b';
            s[i + 4] = 'e';
            s[i + 5] = 'r';
        }
    }

    cout << s << endl;
}
```

---
