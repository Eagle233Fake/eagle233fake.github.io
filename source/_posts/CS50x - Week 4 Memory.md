---
title: CS50x - Week 4 Memory
date: 2024-09-30 12:31:47
tags:
categories: 
- CS50x
---

---

`File I/O` 是真不太懂啊，立个flag慢慢学。

---

## Pointers

- **type \***
- **\*x** // Dereference Operator
- **&x**

### Reasons to Use Pointers

-You can pass variables to functions **by reference**, not just **by copy**. The code you write is cleaner as a result.

- You can use **dynamic memory** (e.g., with malloc). Your programs can now scale their usage of memory according to user behavior.

        #include <cs50.h>
        #include <stdio.h>

        void swap(int *a, int *b);

        int main(void)
        {
            int a = 10;
            int b = 50;

            printf("a is %i, b is %i\n", a, b);
            swap(&a, &b);
            printf("a is %i, b is %i\n", a, b);
        }

        void swap(int *a, int *b)
        {
            int temp = *a;
            *a = *b;
            *b = temp;
        }

### Attentions

- The name of an array is ACTUALLY a pointer to the first element of the array.

- One more annoying thing with those *s. They’re an important
part of both the type name and the variable name.

        ~~~int* px, py, pz;~~~
        int* pa, *pb, *pc;

### Segmentation Fault

- ALWAYS set the value to `NULL`, otherwise you would probably mess up your memory, functions or programs.

---

## File I/O

### Opening & Closing Files

- `fopen` opens a file for future reading/writing.

        FILE *f = fopen("hi.txt", "r");

  - "r", if opening the file in order to read from it,

  - "w", if opening the file in order to write to it, and

  - "a", if opening the file in order to append to it.

  - fopen returns a pointer to "FILE" type.

- `fclose` closes a file.

        fclose(f);

- Always fclose all files you fopen! (Reasons?)

### Reading & Writing

- `fread` reads data from a file into a buffer (a chunk of memory that can temporarily store some data from the file). Allows us to look at some particular pieces of our file and not the entire file all at once. It would **return how many blocks** it has read as an interger.

        fread(buffer, 1, 4, f);

        // To where? / What size? / How many? / From where?
        // `buffer` is a **pointer**.

  - Pointer `f` continues through the file until spins to the end and back to the beginning.

- `fwrite` writes data from a buffer to a file.

- `fprintf`

        fprintf(file, "%c %i\n", c, c);
        // `file` is a **pointer**

---

## Syntax

### `sizeof()`

- 是**运算符**而不是函数。

- 返回一个 size_t 类型的值，表示字节数。

---

## 奇怪的语法

### Bitwise Operations `&`

    (buffer[3] & 0xf0) == 0xe0

    // 看不懂，以后再说。

### `sprintf`

    char *filename = malloc(8 * sizeof(char));
    sprintf(filename, "%03i.jpg", 2);

---

## Defining Custom Types

    typedef char * string;

    struct car
    {
    int year;
    char model[10];
    char plate[7];
    int odometer;
    double engine_size;
    };
    typedef struct car car_t;

    typedef struct car
    {
    int year;
    char model[10];
    char plate[7];
    int odometer;
    double engine_size;
    }
    car_t;

---

## Dynamic Memory Allocation

- This memory comes from a pool of memory know as the **HEAP (堆)**.

- Previously all memory comes from a pool of the memory known as the **STACK (栈zhan4)**.

        // get an integer from the user
        int x = GetInt();
        // array of floats on the stack
        float stack_array[x];
        // array of floats on the heap
        float* heap_array = malloc(x * sizeof(float));

// Put an image here

### `malloc`

    #include <stdlib.h>
    int *x = malloc(sizeof(int)); // How many bytes
    if (x == NULL) 
    {
        // 处理内存分配失败
        return 1;
    }

- 每次都需要使用 `free` 释放内存。

- 最好检查返回值是否是 `NULL` 。

### `valgrind`

    valgrind ./program

### `free`

    free(x);
    // ONLY USE in Dynamic Memory Allocation
    // DO NOT use more then once.

---

## Call Stack

- **The Active Frame**: At the top of the stack while other functions waiting to be the active frames.

  - Process: be **PUSHED onto** the top of the stack and be **POPPED off** the stack.

// image here

---

## File Pointers

| 函数名称   | 功能描述                          |
|------------|-----------------------------------|
| `fgets()`  | Reads a full string from a file. |
| `fputs()`  | Writes a full string to a file.  |
| `fprintf()`| Writes a formatted string to a file. |
| `fseek()`  | Allows you rewind or fast-forward within a file. |
| `ftell()`  | Tells you at what (byte) position you are at within a file. |
| `feof()`   | Tells you whether you’ve read to the end of a file. |
| `ferror()` | Indicates whether an error has occurred in working with a file. |

### `FILE` / `FILE *`

### `fopen()`

- Opens a file and returns a file pointer to it.

- Always check the return value to make sure you don’t get back `NULL`.

        FILE* ptr = fopen(<filename>, <operation>);

| 模式 | 描述                                                                                     |
|------|------------------------------------------------------------------------------------------|
| `"a"`| 以追加模式打开文件。如果文件不存在，则创建新文件；如果文件已存在，数据将被写入文件末尾，而不会覆盖原有内容。 |
| `"w"`| 以写入模式打开文件。如果文件已存在，文件内容将被清空（覆盖）；如果文件不存在，则创建新文件。               |

### `fclose()`

    fclose(<file pointer>);

### `fgetc()`

- Reads and returns the next character from the file pointed to.

- Note: The operation of the file pointer passed in as a parameter MUST BE `“r”` for read, or you will suffer an error.

        char ch = fgetc(<file pointer>);

        char ch;
        while((ch = fgetc(ptr)) != EOF)
        {
            printf(“%c”, ch);
        }
        
- Equal to `cat`.

- 'EOF': The end of the file.

### `fputc()`

- Writes or appends the specified character to the pointed-to file.
- Note: The operation of the file pointer passed in as a parameter must be `“w”` for write or `“a”` for append, or you will suffer an error.

        fputc(<character>, <file pointer>);

        char ch;
        while((ch = fgetc(ptr)) != EOF)
        {
            fputc(ch, ptr2);
        }

- Equal to `cp`.

### `fread()`

    fread(<buffer>, <size>, <qty>, <file pointer>);

    int arr[10];
    fread(arr, sizeof(int), 10, ptr);

    double* arr2 = malloc(sizeof(double) * 80);
    fread(arr2, sizeof(double), 80, ptr);

    char c;
    fread(**&c**, sizeof(char), 1, ptr);

### `fwrite()`

    fwrite(<buffer>, <size>, <qty>, <file pointer>);

    int arr[10];
    fwrite(arr, sizeof(int), 10, ptr);

    double* arr2 = malloc(sizeof(double) * 80);
    fwrite(arr2, sizeof(double), 80, ptr);

    char c;
    fwrite(&c, sizeof(char), 1, ptr);

---
