---
title: CS50x - Week 3 Algorithms
date: 2024-09-28 21:46:57
tags:
---

---

这一周我没有完成 **[Tideman](https://cs50.harvard.edu/x/2024/psets/3/tideman/)** ，原因是 **[Runoff](https://cs50.harvard.edu/x/2024/psets/3/runoff/)** 的代码量就已经把我打出措手不及了，计划于之后补上。

这节课的语法内容并不多，作业其实也只涉及到了 `Struct` 的内容（甚至没有 `Recursion` ？也许在 **[Tideman](https://cs50.harvard.edu/x/2024/psets/3/tideman/)** 中？），重点是对算法概念的理解。

---

## Searching and Sorting / O, Θ, Ω Notation

### Linear Search / Binary Search

| Algorithm     | O         | Ω    |
|---------------|-----------|------|
| Linear Search | O(N)      | Ω(1) |
| Binary Search | Ω(log(N)) | Ω(1) |

- **Binary Search** requires a special condition be met beforehand -- have a **SORTED** array.

### Common Notations

| O           | Ω           |
|-------------|-------------|
| O(1)        | Ω(1)        |
| O(log(N))   | Ω(log(N))   |
| O(N log(N)) | O(N log(N)) |
| O(N)        | Ω(N)        |
| O(N^2)      | Ω(N^2)      |

- 1: Fixed number of steps. (Whatever 2, 3, 10, 114514 or ...)

---

## Sort

| Algorithm      | O           |  Ω          |
|----------------|-------------|-------------|
| Merge Sort     | O(N log(N)) | Ω(N log(N)) |
| Selection Sort | O(N^2)      | Ω(N^2)      |
| Bubble Sort    | O(N^2)      | Ω(N)        |

---

## Structs

    typedef struct 
    {
        string name;
        int votes; 
    }
    candidate;

    candidate president;
    president.name = "Samia";
    president.votes = 10;

    candidate candidates[4];

> 那这边要注意一下，经常会混淆数据类型和变量名称。例如创造了一个叫做 `candidate` 的数据类型，然后定义变量 `candidates` 是 `candidate` 类型的数据。

---

## Recursion

- **Example: Factorial**

- **Base Case** / **Recursive Case**  

- **Call Stack**

- **Multiple Base Cases: The Fibonacci Sequence 斐波那契数列**

- **Multiple Recursive Cases: The Collatz Conjecture 科拉茨猜想**

        int collatz(int n)
        {
        // base case
        if (n == 1)
            return 0;
        // even numbers
        else if ((n % 2) == 0)
            return 1 + collatz(n/2);
        // odd numbers
        else
            return 1 + collatz(3*n + 1);
        }

> 想想高中学过的数列：**递归公式**和**首项**。

---
