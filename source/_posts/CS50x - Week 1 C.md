---
title: CS50x - Week 1 C
categories: 
- CS50x
---

---

## **Data Types**

### `int`

    %i

- Take up **32 bits**.

#### Compare `%i` to `%d`

- 当使用 `%i` 时，如果输入的整数以 `0` 开头，程序会将其视为八进制（base 8）；如果以 `0x` 或 `0X` 开头，则视为十六进制（base 16）。例如：
  - `scanf("%i", &num);` 输入 `010` 会将其视为八进制（8），输入 `0x10` 会将其视为十六进制（16）。
- `%d` 不会进行这样的进制识别，始终将输入视为十进制。

### `unsigned int`

    %u

Take up **32 bits**, only for positive integers.

- **思考如下的代码**

        #include <stdio.h>
        int main(void)
        {
            unsigned int m = 1 << 31;
            printf("%u ", m);
            printf("%u ", (m >> 1));

            int n = 1 << 31;
            printf("%d ", n);
            printf("%d\n", (n >> 1));
        }

    假设输入 n = -1（其二进制补码表示为 11111111111111111111111111111111）：

  - 使用 `unsigned int`：

    m 从 10000000000000000000000000000000 开始，逐位检查并打印出 11111111111111111111111111111111。

  - 使用 `int`：

    m 从 10000000000000000000000000000000 开始，但在右移时，符号位会被保留，导致最终结果不正确。


### `long` / `long int`

    %li

- Take up **64 bits**.

> truncation

### `const int`

    %i

- Declare a variable whose value cannot be changed after it is initialized.

- Always use all capitals.

- Take up **32 bits**.

### `uint8_t`

    %i

- Unsigned integer type with a width of exactly 8 bits.

- In `stdint.h` C99.

- Take up **1 bytes = 8 bits**.

// IN "WEEK 4 MEMORY"

### `int16_t`

// IN "WEEK 4 MEMORY"

### `char`

    %c

- Using single quotes.

- Take up **1 bytes = 8 bits**.

### `float`

    %f

    %.5f // 显示五位小数

- Take up **32 bits**.

### `double`

    %f

- Take up **64 bits**.

#### Compare `%f` to `%lf`

- 在 `printf` 中，使用 `%f` 和 `%lf` 是可以互换的。

- 在 `scanf` 中，必须使用 `%f` 来读取 `float`，使用 `%lf` 来读取 `double`。

### `bool`

    %i

- Printed as an integer.

- Not included in C99.

- Take up **1 bytes = 8 bits**.

### `string`

    %s

### `void`

### `size_t`

    %zu

- 32 位系统: 在 32 位系统上，size_t 通常占用 4 字节（32 位）。

- 64 位系统: 在 64 位系统上，size_t 通常占用 8 字节（64 位）。

- `sizeof()` 返回的数据类型。

// IN "WEEK 4 MEMORY"

### Translate into other types

    int x
    (float) x

---

## **Conditionals**

### `?:` (Question Mark Syntax / Ternary  Operator)

    int x;
    if (expr)
    {
        x = 5;
    }
    else
    {
        x = 6;
    }

to

    int x = (expr) ? 5 : 6;

### `switch`

    switch (x)
    {
    case 1:
        // Code to execute if variable == value1
        break;
    case 2:
        // Code to execute if variable == value2
        break;
    // Add more cases as needed
    default:
        // Code to execute if variable doesn't match any case
        break;
    }

---

## **Loops**

### `do-while`

    do
    {
        
    }
    while (boolean-expr);

- Run **at least one time** (Compared to `while`).

### `for`

    for (start; expr; increment)
    {

    }

---

## **Command Line**

### 快捷键

- `Ctrl + L` 清屏
- `Ctrl + U` 清除当前行
- `Ctrl + A` 跳转到开头
- `Ctrl + E` 跳转到结尾

### `pwd`

- Give a **prompt** that reminds you where you are.

### `cp`

`cp <source> <destination>`

> recursion / recursive / recursively

- To copy an entire directory, use `cp -r <directory> <directory>`.

### `rm`

- To force rm, use `rm -f <source>`.

- To remove an entire directory, use `rm -r(f) <directory>`.

### `mv`

- Example:
    `cp greddy.c greedy.c`
    `rm greddy.c`
    to
    `mv greddy.c greedy.c`
    The same effect.

### `man`

- Manual

### Other commands

`chmod` `ln` `touch` `rmdir` `diff` `sudo` `clear` `telnet`

---

## **Magic Numbers**

### Preprocessor Directive (macro)

- To create symbolic constants.

`#define NAME REPLACEMENT`

- Always use all capitals.

---

## VSCode

- `Ctrl + Shift + P` Command

- `Ctrl + B` Explorer

- `Ctrl + 反引号` Terminal // 在``里面怎么就打不出反引号来？？？

---
