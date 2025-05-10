---
title: Programming Ability Training - All Codes
date: 2025-03-12 13:24:36
tags:
categories: 
- In Class Practices
---

## Weel 1

### A Problem A+B (Big Integer)

```cpp
#include <iostream>
#include <string>
#include <algorithm>

using namespace std;

int main(void)
{
    string a, b;
    while (cin >> a >> b)
    {
        string s;
        reverse(a.begin(), a.end()); // 翻转字符串功能
        reverse(b.begin(), b.end());

        int length = max(a.length(), b.length());
        int carry = 0;
        for (int i = 0; i < length; i++)
        {
            int digit_a = i < a.length() ? a[i] - '0' : 0;
            int digit_b = i < b.length() ? b[i] - '0' : 0;
            int sum = digit_a + digit_b + carry;
            carry = sum / 10;
            sum %= 10;
            s.push_back(sum + '0');
        }

        if (carry)
        {
            s.push_back('1');
        }
        reverse(s.begin(), s.end());

        cout << s << endl;
    }
}
```

### B 几位数

```c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char s[500000];
    while (scanf("%s", s) != EOF)
    {
        int length = strlen(s);
        printf("%d\n", length);
    }
}
```

### C 随机排序

```cpp
#include <bits/stdc++.h>
#include <cctype>

using namespace std;

int prime[26];

bool cmp(const string& a, const string& b)
{
    int length = min(a.length(), b.length());
    for (int i = 0; i < length; i++)
    {
        if (a[i] != b[i])
        {
            if (a[i] - b[i] == 'a' - 'A')
            {
                return b[i] > a[i];
            }
            else if (b[i] - a[i] == 'a' - 'A')
            {
                return a[i] < b[i];
            }
            return prime[toupper(a[i]) - 'A'] < prime[toupper(b[i]) - 'A'];
        }
    }
    return a.length() < b.length();
}

int main(void)
{
    string s;
    while (cin >> s)
    {
        for (int i = 0; i < s.length(); i++)
        {
            prime[s[i] - 'A'] = i;
        }

        getchar();

        string words;
        getline(cin, words); // 获取整行

        vector<string> v;

        int index = 0;
        for (int i = 0; i <= words.length(); i++)
        {
            if (i == words.length() || words[i] == ' ')
            {
                string word = words.substr(index, i - index);
                v.push_back(word);
                index = i + 1;
            }
        }

        sort(v.begin(), v.end(), cmp);
        for (string &word : v)
        {
            cout << word << " ";
        }
        cout << endl;
    }
}
```

### D 行数据的排序

```cpp
#include <bits/stdc++.h>

using namespace std;

bool cmp(const vector<int>& a, const vector<int>& b)
{
    int length = min(a.size(), b.size());
    for (int i = 0; i < length; i++)
    {
        if (a[i] != b[i])
        {
            return a[i] > b[i];
        }
    }
    return a.size() < b.size();
}

int main(void)
{
    int n;
    cin >> n;

    for (int i = 0; i < n; i++)
    {
        int m;
        cin >> m;
        getchar();

        if (m == 1)
        {
            string s;
            getline(cin, s);
            int length = s.length();
            cout << s.substr(0, length - 3) << endl;
        }

        else
        {
            vector<vector<int>> v;
            for (int i = 0; i < m; i++)
            {
                string s;
                vector<int> temp;
                getline(cin, s);
                s = s.substr(0, s.length() - 3);

                int index = 0;
                for (int i = 0; i <= s.length(); i++)
                {
                    if (s[i] == ' ' || i == s.length())
                    {
                        temp.push_back(stoi(s.substr(index, i - index)));
                        index = i + 1;
                    }
                }

                v.push_back(temp);
            }
            sort(v.begin(), v.end(), cmp);
            for (const vector<int>& vec : v)
            {
                for (const int& v : vec)
                {
                    cout << v << " ";
                }
                cout << endl;
            }
        }
    }
}
```

### E 按数据中1的位数排序

```cpp
#include <bits/stdc++.h>
#include <bitset>

using namespace std;

bool cmp(const long long &a, const long long &b) {
    bitset<120> binaryA(a); // 转为 binary
    bitset<120> binaryB(b);
    if (binaryA.count() == binaryB.count()) { // 方法 count() 统计1的个数
        return a < b;
    }
    return binaryA.count() > binaryB.count();
}

int main(void) {
    int n;
    cin >> n;
    getchar();
    for (int i = 0; i < n; i++) {
        cout << "case #" << i << ":" << endl;
        int m;
        cin >> m;

        vector<long long> v;
        for (int j = 0; j < m; j++) {
            long long num;
            cin >> num;
            v.push_back(num);
        }

        sort(v.begin(), v.end(), cmp);

        for (long long l : v) {
            cout << l << " ";
        }
        cout << endl;
    }
}
```

### F 字符频率

```cpp
#include <bits/stdc++.h>

using namespace std;

vector<double> f(26);

bool cmp(const char &a, const char &b) {
    char m = toupper(a);
    char n = toupper(b);
    if (m == n) {
        return a > b; // 1为a在前，0为b在前
    }

    int indexA = m - 'A';
    int indexB = n - 'A';
    if (f[indexA] == f[indexB]) {
        return m < n;
    }
    return f[indexA] > f[indexB];
}

int main(void) {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        cout << "case #" << i << ":" << endl;
        for (int j = 0; j < 26; j++) {
            cin >> f[j];
        }

        string s;
        cin >> s;
        
        sort(s.begin(), s.end(), cmp);

        cout << s << endl;
    }
}
```

### G 极坐标排序

```cpp
#include <iostream>
#include <cmath>
#include <algorithm>
#include <vector>
#include <iomanip> 

using namespace std;

struct Point
{
    double p;
    double t;
};

bool cmp(const Point& a, const Point& b);

int main(void)
{
    int c;
    cin >> c;

    for (int i = 0; i < c; i++)
    {
        cout << "case #" << i << ":" << endl;
        int n;
        cin >> n;

        vector<Point> points(n);
        for (int j = 0; j < n; j++)
        {
            double x, y;
            cin >> x >> y;

            points[j].p = sqrt(x * x + y * y);

            points[j].t = atan2(y, x); // 计算弧度，返回范围是-pai到pai
            if (points[j].t < 0)
            {
                points[j].t += 2 * M_PI;
            }
        }

        sort(points.begin(), points.end(), cmp);

        for (const Point &p : points)
        {
            cout << fixed << setprecision(4); // 保留四位
            cout << "(" << p.p << "," << p.t << ")" << endl;
        }
    }
}

bool cmp(const Point& a, const Point& b)
{
    if (a.t == b.t)
    {
        return a.p > b.p;
    }
    return a.t < b.t;
}
```

### H 字串非重复字符数排序

```cpp
#include <bits/stdc++.h>

using namespace std;

int times(string a) {
    vector<bool> t(26, false); // 注意初始化格式
    for (char s : a) {
        t[s - 'A'] = true;
    }

    int times = 0;
    for (int i = 0; i < 26; i++) {
        if (!t[i]) {
            times++;
        }
    }
    return times;
}

bool cmp(const string &a, const string &b) {
    int timesA = times(a);
    int timesB = times(b);

    if (timesA == timesB) {
        return a < b;
    }
    return timesA < timesB;
}

int main(void) {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        cout << "case #" << i << ":" << endl;
        int m;
        cin >> m;

        vector<string> v;
        for (int j = 0; j < m; j++) {
            string s;
            cin >> s;
            v.push_back(s);
        }

        sort(v.begin(), v.end(), cmp);
        for (string s : v) {
            cout << s << endl;
        }
    }
}
```

### I KL排序

```cpp
#include <bits/stdc++.h>

using namespace std;

struct KL {
    double value;
    int id;
};

bool cmp(const KL &a, const KL &b) {
    if (fabs(a.value - b.value) < 1e-7) {
        return a.id < b.id;
    }
    return a.value < b.value;
}

int main(void) {
    int m;
    cin >> m;
    for (int i = 0; i < m; i++) {
        cout << "case #" << i << ":" << endl;
        int k, n;
        cin >> k >> n;

        vector<vector <int>> v;
        for (int j = 0; j < n + 1; j++) {
            vector<int> temp;
            for (int c = 0; c < k; c++) {
                int tempNumber;
                cin >> tempNumber;
                temp.push_back(tempNumber);
            }

            v.push_back(temp);
        }

        vector<int> M(n + 1, 0);
        for (int j = 0; j < n + 1; j++) {
            for (int c = 0; c < k; c++) {
                M[j] += v[j][c];
            }
        }

        vector<vector <double>> p;
        for (int j = 0; j < n + 1; j++) {
            vector<double> temp;
            for (int c = 0; c < k; c++) {
                double tempNumber = (v[j][c] + (double)1.0 / k) / (M[j] + 1);
                temp.push_back(tempNumber);
            }

            p.push_back(temp);
        }

        vector<KL> kl;
        for (int j = 1; j < n + 1; j++) {
            double temp = 0;
            for (int c = 0; c < k; c++) {
                if (p[0][c] > 0 && p[j][c] > 0) {
                    temp += p[0][c] * log(p[0][c] / p[j][c]);
                }
            }
            if (fabs(temp) < 1e-7) {
                temp = 0.0000;
            }
            KL tempKL;
            tempKL.id = j;
            tempKL.value = temp;
            kl.push_back(tempKL);
        }

        sort(kl.begin(), kl.end(), cmp);

        for (KL tp : kl) {
            cout << tp.id << " " << fixed << setprecision(4) << tp.value << endl; // 回忆一下 setprecision
        }
    }
}
```

## Week 2

### J 内存显示

```cpp
#include <bits/stdc++.h>

using namespace std;

bool isDecimal(string b) {
    // return a != floor(a); // floor 返回的是整数部分
    for (int i = 0; i < b.length(); i++) {
        if (b[i] == '.') {
            return true;
        }
    }
    return false;
}

int main(void) {
    string a;
    char *b;
    while (cin >> a) {
        double m = stod(a);
        if (isDecimal(a)) {
            b = (char *)&m;
            for (int i = 0; i < 8; i++) {
                printf("%02x ", (unsigned char)*b); // %02x 用于输出每个字节的十六进制表示
                b++;
            }
        } else {

            int c = (int)m;
            b = (char *)&c;
            for (int i = 0; i < 4; i++) {
                printf("%02x ", (unsigned char)*b);
                b++;
            }
        }

        cout << endl;
    }
}
```

### K 二进制倒置

```cpp
#include <bits/stdc++.h>

using namespace std;

// 这个考试一定要带进去，根本做不来
string convert(int n, string s, int m) {
    // 将输入字符串从n进制转换为十进制
    vector<int> decimal_digits;
    for (char c : s) {
        int carry = c - '0';
        for (int &d : decimal_digits) {
            d = d * n + carry;
            carry = d / 10;
            d %= 10;
        }
        while (carry) {
            decimal_digits.push_back(carry % 10);
            carry /= 10;
        }
    }

    // 将十进制转换为m进制
    string result;
    vector<int> value(decimal_digits);
    do {
        int carry = 0;
        for (int i = value.size() - 1; i >= 0; --i) {
            carry = carry * 10 + value[i];
            value[i] = carry / m;
            carry %= m;
        }
        result.push_back('0' + carry);
        while (value.size() > 1 && value.back() == 0) {
            value.pop_back();
        }
    } while (!value.empty() && !(value.size() == 1 && value[0] == 0));

    reverse(result.begin(), result.end());
    return result;
}

int main(void) {
    int n = 0;
    cin >> n;
    for (int i = 0; i < n; i++) {
        cout << "case #" << i << ":" << endl;
        string s;
        cin >> s;
        string s2 = convert(10, s, 2);
        reverse(s2.begin(), s2.end());

        while (s2.size() > 1 && s2[0] == '0') {
            s2 = s2.substr(1);
        }

        string s3 = convert(2, s2, 10);
        cout << s3 << endl;
    }
}
```

### L 平衡三进制

```cpp
#include <bits/stdc++.h>

using namespace std;

int main(void) {
    string s;
    cin >> s;
    int index = s.find('.');
    if (index == -1) {
        index = s.length();
    }

    long long a = 0;
    // 计算整数部分
    long long power = 1;
    for (int i = index - 1; i >= 0; i--) {
        if (s[i] == '1') {
            a += power;
        } else if (s[i] == '2') {
            a -= power;
        }
        power *= 3;
    }

    // 计算小数部分
    long long b = 0, c = 1;
    for (int i = index + 1; i < s.length(); i++) {
        if (s[i] == '1') {
            b = b * 3 + 1;
            c *= 3;
        } else if (s[i] == '2') {
            b = b * 3 - 1;
            c *= 3;
        } else {
            b *= 3;
            c *= 3;
        }
    }

    // 带分数怎么计算：取决于符号位

    if (a > 0 && b < 0) {
        b = c + b;
        a -= 1;
    } else if (a < 0 && b > 0) {
        b = c - b;
        a += 1;
    }

    long long gcda = gcd(abs(b), c);
    b /= gcda;
    c /= gcda;

    if (index == s.length()) {
        cout << a << endl;
        return 0;
    }
    if (a == 0) {
        cout << b << " " << c << endl;
    } else {
        cout << a << " " << abs(b) << " " << c << endl;
    }
}
```

### M 神秘信息

```cpp
#include <bits/stdc++.h>

using namespace std;

long long convert(string s) {
    vector<int> digit;
    unordered_map<char, int> m; // HashTable
    int base = 0;
    for (char c  : s) {
        if (m.find(c) == m.end()) {
            if (base == 0) {
                m[c] = 1;
            } else if (base == 1) {
                m[c] = 0;
            } else {
                m[c] = base;
            }
            base++;
        }
        digit.push_back(m[c]);
    }

    base = max(base, 2); // 不是一进制
    long long result = 0;
    for (int i : digit) {
        result = result * base + i;
    }
    return result;
}

int main(void) {
    int n;
    cin >> n;
    cin.ignore();
    for (int i = 0; i < n; i++) {
        cout << "case #" << i << ":" << endl;
        string a;
        cin >> a;
        cout << convert(a) << endl;
    }
}
```

### N 数据密度

```cpp
#include <bits/stdc++.h>

using namespace std;

int main(void) {
    int n;
    cin >> n;
    cin.ignore();  // 清除换行符
    for (int i = 0; i < n; i++) {
        string a;
        getline(cin, a);

        int m = 0;
        int n = a.length() * 8;
        for (int h = 0; h < a.length(); h++) {
            int c = a[h];
            bitset<8> d(c);
            m += d.count();
        }

        int gcda = gcd(m, n); // 为什么要先提取出来？
        m /= gcda; // 最小公因数
        n /= gcda;
        cout << m << "/" << n << endl;
    }
}
```
