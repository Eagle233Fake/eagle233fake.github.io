---
title: CS50x - Week 6 Python
date: 2024-11-18 14:48:51
tags:
---

---

时隔一个月重拾 CS50x 的学习啦！其实我觉得记语法的笔记意义不是特别大，所以这里就偷懒了2333，还是多练习比较管用。

---

## 语法特性

### For Loop

思考一下当需要隔行遍历的时候， `for i in range(n)` 和 `for (int i = 0; i < n; i++)` 区别在哪？

    # sentimental-credit/credit.py

    from cs50 import get_string

    n = get_string("Number: ")

    sum = 0

    """
    for i in range(len(n)):
        i += 1
        digit = int(n[len(n) - 1 - i]) * 2
        if digit > 9:
            digit = 1 + digit - 10
        sum += digit

    for i in range(len(n)):
        sum += int(n[len(n) - 1 - i])
        i += 1
    """
    # Think why it doesn't work?

    for i in range(len(n)):
        digit = int(n[len(n) - 1 - i])
        if i % 2 == 1:
            digit *= 2
            if digit > 9:
                digit = 1 + digit - 10
            sum += digit
        else:
            sum += int(n[len(n) - 1 - i])

    if sum % 10 == 0:
        if n[0] == "5" and (n[1] == "1" or n[1] == "2" or n[1] == "3" or n[1] == "4" or n[1] == "5") and len(n) == 16:
            print("MASTERCARD")
        elif n[0] == "4" and (len(n) == 13 or len(n) == 16):
            print("VISA")
        elif n[0] == "3" and (n[1] == "4" or n[1] == "7") and len(n) == 15:
            print("AMEX")
        else:
            print("INVALID")
    else:
        print("INVALID")
