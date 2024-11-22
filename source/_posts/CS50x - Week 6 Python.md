---
title: CS50x - Week 6 Python
date: 2024-11-18 14:48:51
tags:
categories: 
- CS50x
---

---

时隔一个月重拾 CS50x 的学习啦！其实我觉得记语法的笔记意义不是特别大，所以这里就偷懒了2333，还是多练习比较管用。

ALL THE COURSES TAKE IN CS50X NOW ARE USING **PYTHON 3**.

---

## Compare with `C`

- There is no POINTERS or char * in Python.

- Automatically adjust the data types so you needn't to initialize a variable.

- `''` `""` would be exactly the same. You can just kind of alternate back and forth between using single quotes on the outside, double quotes on the inside.

- Use `or` `and` instead of `||` `&&` .

- `elif`

- Comment: `#` or `'''` `"""`

- `i++` is unavailable.

- Indentation is important while C didn't care it (From left to right).

- Access the string: `text[i]` .

---

## Dot Notation

### `strip()`

    text = input("...")
    text.strip()

- Take out beginning and trailing whitespace characters.

### `lower()` `upper()`  `capitalize()`

    text.lower()      # All to lowercase.
    text.capitalize() # First character only to uppercase while others to lowercase.

- Remember to reassign: `text = text.lower()` .

### Object-Oriented Programming

- `str` itself is a object that has its own attributes and methods.

---

## Loops

### `for`

- 遍历 `str`

        for c in text:
            print(c)

- 遍历 `list`

        words = text.split() // 拆分为单词，获得一个列表。
        for word in words:
            print(word)

- 步长

        for x in range(0, 100, 2)
        # From 0 to 99 step by 2.

- Iterable

- 思考一下当需要隔行遍历的时候， `for i in range(n)` 和 `for (int i = 0; i < n; i++)` 区别在哪？

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

- `[2:4]`

        for word in text.split()[2:4]: # 列表第2-3个的元素，第4个不包含。
            print(word)

### `while`

---

## `print()`

    print(text, end="")

- `end` set `\n` as default.

---

## Conditionals `if`

    if "g" in word:
        print(word)

- The Ternary Operator

        letters_only = True if input().isalpha() else False

---

## Dictionaries

- Python also has built in support for dictionaries, allowing you to specify list indices with words or phrases (keys), instead of integers, which you were restricted to in C.

        pizzas = {
            "cheese": 9,
            "pepperoni": 10,
            "vegetable": 11,
            "buffalo chicken": 12
        }

- Key / Value

        book = dict()
        book["title"] = "Corduroy"
        book["author"] = "Don Freeman"
        print(book["title"])

        # What if...
        '''
        print(book["Corduroy"])
        '''
        # KeyError: 'Corduroy'

        book = {
            "title": "Corduroy"
            "author": "Don Freeman"
        }

- Iterate

        for pie in pizzas:
            # use pie in here as a stand-in for "i"

        for pie, price in pizzas.items():
        # Convert the dict into a list but the order would be disturbed.
            print("A whole {} pizza costs ${}".format(pie, price))
            print("A whole " + pie + " pizza costs $" + str(price))

---

## Lists

- Not fixed in size.

        book = [{"title": "Goodnight Moon", ...}, 
                {"title": "Corduroy", ...},
                {"title": "Curious George", ...}]

- 实现：

        books = []
        # or books = list()

        for i in range(3):
            book = dict()
            book["author"] = input("Enter an author: ")
            book["title"] = input("Enter a title: ")
            books.append(book)

        for book in books:
            print(f"{book['author']} wrote {book['title']}.")

        # {Key: Value, ...}
        # {Key: Value, ...}
        # {Key: Value, ...}

- 只返回 `Keys`

            print(book.keys())

        # dict_keys(['author', 'title'])

### Explicitly Created List

    nums = [x for x in range(500)]

### `append()` `insert()`

    nums.append(5)

    nums.insert(4, 5)
    # In the forth position insert 5.

    nums[len(nums):] = [5]

---

## `Tuples` aka 元组

- `Tuples`s are ordered, immutable sets of data; they are great for associating collections of data, sort of like a struct in C, but where those values are unlikely to change.

        presidents = [
            ("George Washington", 1789),
            ("John Adams", 1797),
            ("Thomas Jefferson", 1801),
            ("James Madison", 1809)
        ]

- Iterable

        for prez, year in presidents:
            print("In {1}, {0} took office".format(prez, year))

        '''
        In 1789, George Washington took office
        In 1797, John Adams took office
        In 1801, Thomas Jefferson took office
        In 1809, James Madison took office
        '''

---

## Libraries and Modules

### `.csv`

- 全部导入

        import csv
        csv.DictReader(...)

- 导入特定函数

        from csv import DictReader
        DictReader(...)

### File Reading and Writing

    with open(FILENAME) as file:
    # with open(FILENAME, "r") as file: different modes.
        text = file.read() # Now `text` is a str including all the content of `FILENAME`.
        file_reader = csv.DictReader(file) # Now `file_reader` is a ITERATOR.

        for row in file_reader: # Now `row` is a dict.
            ...

- `with` would automatically close the file outside of the sentence.

---

## Functions

- If you wish to define main nonetheless (and you might want to!), you must at the very **end** of your code have:

        if __name__ == "__main__":
            main()

### `def`

    def square(x):
        return x ** 2
        # x ^ 2
    
    print(square(5))

---

## Objects

    ~~function(object);~~
    object.method()

### `class`

    class Student():
        def __init__(self, name, id):
            self.name = name
            self.id = id
        def changeID(self, id):
            self.id = id
        def print(self):
            print("{} – {}".format(self.name, self.id))

        jane = Student("Jane", 10)
        jane.print()
        jane.changeID(11)
        jane.print()

---

## Including Files

    import cs50

    cs50.get_int()
    cs50.get_float()
    cs50.get_string()

---

## ...Wired?

- You can also make your programs look a lot more like C programs when they execute by adding a shebang to the top of your Python files, which automatically finds and executes the interpreter for you.

        #!/usr/bin/env python3

- If you do this, you need to change the permissions on your file as well using the Linux command chmod as follows:

        chmod a+x file

---
