---
title: CS106L - 01 Streams I
date: 2024-11-21 21:36:54
tags:
categories: 
- CS106L
---

---

- 视频链接：

**[CS 106L Fall 2019 - Lecture 1: Streams I (Video)](https://www.youtube.com/watch?v=nmXAy_59twk&list=PLCgD3ws8aVdolCexlz8f3U-RROA0s5jWA&index=4)**

**[CS 106L Fall 2019 - Lecture 1: Streams I (Screencast)](https://www.youtube.com/watch?v=_WvkljZzGug&list=PLCgD3ws8aVdolCexlz8f3U-RROA0s5jWA&index=3)**

---

---

- Almost the **oldest** library in CPP.

- `stream` are very very very very messy. (So in CS106B use `stanford library`)

- 用于与外界交互。(Console & Keyboard and Files in CS106L)

- `<<`: output while `>>`: input

---

## `stringstream`

`istringstream` 和 `ostringstream` 是 C++ 标准库中用于处理字符串流的两个类，分别用于输入和输出操作。以下是它们的主要区别和特点：

### `istringstream`

1. **定义**:

   - `istringstream` 是一个输入字符串流类，允许从字符串中读取数据。

2. **主要功能**:

   - 主要用于将字符串解析为其他数据类型（如整数、浮点数等）。

   - 提供与标准输入流（如 `cin`）相似的接口，可以使用提取运算符（`>>`）从字符串中读取数据。

### `ostringstream`

1. **定义**:

   - `ostringstream` 是一个输出字符串流类，允许将数据写入字符串。

2. **主要功能**:

   - 主要用于将其他数据类型格式化为字符串。

   - 提供与标准输出流（如 `cout`）相似的接口，可以使用插入运算符（`<<`）将数据写入字符串。

### 实现

#### `ostringstream` 实例

```cpp
#include <sstream>
#include <iostream>

using namespace std;

int main(void)
{
    ostringstream oss("Ito En Green Tea "); // 初始化的位置永远在最前.
    cout << oss.str() <<endl; // 注意到本身 `oss` 并不是一个 `str` 类型，需要转换才能 `cout` .
    
    oss << 16.9 << " Ounce "; // " Ounce " 实际上并不是一个 str （为什么？），但也可以写成 string(" Ounce ")

    return 0;
}
```

```text
16.9 Ounce n Tea // 这时候位置指向 `n` 的前面，前面的内容会被覆盖.
```

#### `stringstream::ate`

```cpp
#include <sstream>
#include <iostream>

using namespace std;

int main(void)
{
    ostringstream oss("Ito En Green Tea ", stringstream::ate); // 指向最后面
    cout << oss.str() <<endl;
    
    oss << 16.9 << " Ounce ";

    return 0;
}
```

```text
Ito En Green Tea 16.9 Ounce  // 这时候位置指向 `n` 的前面，前面的内容会被覆盖.
```

#### 再覆盖一次

```cpp
#include <sstream>
#include <iostream>

using namespace std;

int main(void)
{
    ostringstream oss("Ito En Green Tea ");
    cout << oss.str() <<endl;
    
    oss << 16.9 << " Ounce ";
    cout << oss.str() <<endl;

    oss << "(Pack of " << 12 << ")";
    cout << oss.str() <<endl;

    return 0;
}
```

```text
Ito En Green Tea 
16.9 Ounce n Tea
16.9 Ounce (Pack of 12) // 接下去覆盖
```

- 缓存区 `buffer` 足够巨大，你不需要关心底层细节。

#### 再次展示 `stringstream::ate`

```cpp
#include <sstream>
#include <iostream>

using namespace std;

int main(void)
{
    ostringstream oss("Ito En Green Tea ", stringstream::ate);
    cout << oss.str() <<endl;
    
    oss << 16.9 << " Ounce ";
    cout << oss.str() <<endl;

    oss << "(Pack of " << 12 << ")";
    cout << oss.str() <<endl;

    return 0;
}
```

```text
Ito En Green Tea 
Ito En Green Tea 16.9 Ounce 
Ito En Green Tea 16.9 Ounce (Pack of 12)
```

#### 直接 `oss << 16.9`

直接的数据转换是有用的。我们可以将 `16.9` 初始化为 `double size` ，例如：

```cpp
#include <sstream>
#include <iostream>

using namespace std;

int main(void)
{
    ostringstream oss("Ito En Green Tea ", stringstream::ate);
    cout << oss.str() <<endl;
    
    double size = 16.9;
    oss << size << " Ounce ";
    cout << oss.str() <<endl;

    oss << "(Pack of " << 12 << ")";
    cout << oss.str() <<endl;

    return 0;
}
```

#### `istringstream` 实例

```cpp
#include <sstream>
#include <iostream>

using namespace std;

int main(void)
{
    ostringstream oss("Ito En Green Tea ");
    
    oss << 16.9 << " Ounce ";

    oss << "(Pack of " << 12 << ")";

    istringstream iss(oss.str());
    double amount; // 不能使用 `auto` ：你必须让编译器明白这是一个高精度浮点数还是一个字符串，编译器并不会自动识别。
    string unit;

    iss >> amount >> unit; // Order and variable types are important.

    cout << "Now each bottle is sold as: " << amount / 2 << " " << unit << endl;

    return 0;
}
```

```text
Now each bottle is sold as: 8.45 Ounce
```

#### Manually Reposition

```cpp
#include <sstream>
#include <iostream>

using namespace std;

int main(void)
{
    ostringstream oss("Ito En Green Tea ");

    oss << 16.9;

    fpos pos = oss.tellp(); // 类似于整型的数据类型，给其赋值为当前的位置.
    
    pos += streamoff(3); // Offset of 3. 不能直接加3. 可以偏移负数但不能超出范围 (Freeze).
    oss.seekp(pos); // 移动到三个位置之后。

    oss << "Black";

    // oss.seekp(0); //这会返回到最开头.
    cout << oss.str() << endl;

    return 0;
}
```

```text
16.9 En Black Tea
```

- 有如下的函数：

    ```cpp
    // get position:
    oss.tellp();    iss.tellg();

    // set position
    oss.seekp(pos); iss.seekg(pos);

    // create offset
    streamoff(n)
    ```

---

## `stringToInterger` Function By Using `sstream`

```cpp
#include <sstream>

int stringToInterger(const string& str)
{
    istringstream iss(str);
    int value;
    iss >> value;
    return value;
}
```

这段代码巧妙运用了 `istringsteam` 在每一个空格前获取一段文字的功能，并且能直接转换为相应的变量类型。

输入是 `"lol"` `""` `"4.6"` `"lol4.6"` `"lol 4.6"` `"lol4"` ，输出分别是什么？

```text
0 0 4 0 0 0
```

### State Bits

1. **`goodbit`**:

   - Indicates that the stream is in a good state. This means that no errors have occurred, and the stream is ready for input or output operations.

   - You can check this state using `stream.good()`.

2. **`eofbit`**:

   - Stands for "end-of-file" bit. This bit is set when the end of the input stream has been reached. It indicates that there are no more data to read.
  
   - You can check this state using `stream.eof()`.

3. **`failbit`**:

   - This bit is set when a logical error occurs during an input or output **operation**. For example, trying to read data of the wrong type or failing to read data.
  
   - You can check this state using `stream.fail()`.

4. **`badbit`**:

   - Indicates a serious error has occurred, such as a failure to read from or write to the stream. This could be due to hardware failure or other critical issues.
  
   - e.g. The file you are reading from suddenly is deleted.

   - You can check this state using `stream.bad()`.

- **F** and **E** are normally the ones you will be checking.
  
- Rarely be checking **G**.

#### `printStateBits`

```cpp
void printStateBits(istream& s)
{
    cout << "State bits: ";

    cout << (s.good() ? "G" : "-");
    cout << (s.fail() ? "F" : "-");
    cout << (s.eof() ? "E" : "-");
    cout << (s.bad() ? "B" : "-");

    cout << endl;
}

int stringToInterger(const string& str)
{
    istringstream iss(str);

    printStateBits(iss);

    int value;
    iss >> value;

    printStateBits(iss);

    return value;
}
```

这段代码将 `iss` 先检查状态，再用 `>>` 提取 `int` ，最后再检查状态。

输入是 `"5"` `"5.2"` `"lol"` `"\n"` `""` ，输出分别是什么？

```text
G--- / --E-
G--- / G--- // `5`被提取，后来被检查的是`.`
G--- / -F--
G--- / -F--
G--- / -FE- //这意味着 `EOF` 既意味着F也意味着E。
```

- `cout` ALWAYS print out the entire string.

### 更好的实现

```cpp
int stringToInterger(const string& str)
{
    istringstream iss(str);

    int value;
    iss >> value;

    if (iss.fail() || !iss.eof()) // 检查不是‘ “是 EOF"和“不是 FAIL” ’，即检查后面还有没有没处理的东西或者读取失败了。
    {
        throw domain_error("stringToInteger:..."); // 这个时候不直接 `return` ，因为这样会返回一个有实际意义的数值，而不是报错。
    }

    return value;
}
```

输入是 `"5"` `"5.2"` `"lol"` `"\n"` ，输出分别是什么？

### 第三个实现方法

```cpp
int stringToInterger(const string& str)
{
    istringstream iss(str);

    int result;
    iss >> result;
    if (iss.fail())
    {
        throw domain_error(_);
    }

    char remain;
    iss >> remain; // 将 `int` 之后的东西输入 `remain` （如果有的话）。
    if (!iss.fail())
    {
        throw domain_error(_);
    }

    return result;
}
```
