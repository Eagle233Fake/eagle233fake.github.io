---
title: CS50x - Week 5 Data Structures
date: 2024-10-12 15:47:02
tags:
categories: 
- CS50x
---

---

温馨提示：Lecture 56分钟开始的部分会非常让人费解。如果你感到理解不能，请尝试用纸笔复现代码功能的示意图（就像 Slide 上的一样）。

---

## Inserting and Unloading a Linked List

### Nodes

    typedef struct node
    {
        char *phrase;
        struct node *next;
    }
    node;

### Creating a Linked List

    node *list = NULL;
    node *n = malloc(sizeof(node));
    if (n == NULL)
    {
        return 1;
    }
    n->phrase = "Hi!";
    n->next = NULL;
    list = n;

### Inserting Nodes

    n = malloc(sizeof(node));
    n->phrase = "Hey!";
    n->next = list;
    list = n;

// image

    free(list);
    // What happen? image

    node *ptr = list->next;
    free(list);
    list = ptr;
    ptr = list->next;
    free(list);
    list = ptr;

---

## Hash Table

### `rand()`

    #include <stdlib.h>
    #include <time.h>
    
    srand(time(0));
    rand() % 2 // 返回余数1或者2。
    
---
