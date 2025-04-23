---
title: '[Algorithms] LeetCode 707. 设计链表'
date: 2025-04-08 14:19:45
tags:
categories: 
- Algorithms
- LinkedList
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 707. 设计链表](https://leetcode.cn/problems/design-linked-list/)

## 单链表

链表问题不大，cpp语法问题很大，构造函数都不会写。
`MyLinkedList`是构造函数，变量不用类型是因为在私有里面已经声明过了。相当于`this->_size` `this->_dummyHead`

注意点：

- 成员结构体类型定义（内部类定义）

    ```cpp
    struct LinkedList {
        int val;
        LinkedList *next;
        LinkedList(int val, LinkedList *next) : val(val), next(next){}
        LinkedList(int val) : val(val), next(nullptr){}
        LinkedList() : val(0), next(nullptr){}
    }; 
    ```

- 构造函数的写法(有参、无参构造)
  
    ```cpp
    LinkedList(int val, LinkedList *next) : val(val), next(next){}

    MyLinkedList() {
        _dummyHead = new LinkedList(0);
        _size = 0;
    }
    ```

- 声明成员变量

    ```cpp
    private:
        int _size;
        LinkedList *_dummyHead;
    ```

- 区别

    ```cpp
    int val;               // ✅ 是结构体 LinkedList 的成员变量
    LinkedList *next;      // ✅ 也是结构体 LinkedList 的成员变量

    struct LinkedList {...};// ❌ 不是成员变量，是“类型定义”
    ```

三刷：结构体写在构造函数的外面，注意是要走到`index`，添加节点的分支语句要`return`，`index`和`_size`的关系。

```cpp
class MyLinkedList {
public:

    struct LinkedList {
        int val;
        LinkedList *next;
        LinkedList(int val, LinkedList *next) : val(val), next(next){}
        LinkedList(int val) : val(val), next(nullptr){}
        LinkedList() : val(0), next(nullptr){}
    };

    MyLinkedList() {
        _dummyHead = new LinkedList(0);
        _size = 0;
    }
    
    int get(int index) {
        LinkedList *p = _dummyHead->next;
        if (index >= _size) {
            return -1;
        }
        for (int i = 0; i < index; i++) {
            p = p->next;
        }
        return p->val;
    }
    
    void addAtHead(int val) {
        _dummyHead->next = new LinkedList(val, _dummyHead->next);
        _size++;
    }
    
    void addAtTail(int val) {
        LinkedList *p = _dummyHead;
        while (p->next != nullptr) {
            p = p->next;
        }
        p->next = new LinkedList(val);
        _size++;
    }
    
    void addAtIndex(int index, int val) {
        if (index == _size) {
            addAtTail(val);
            return;
        }
        if (index == 0) {
            addAtHead(val);
            return;
        }
        if (index > _size) {
            return;
        }

        LinkedList *p = _dummyHead;
        for (int i = 0; i < index; i++) {
            p = p->next;
        }
        p->next = new LinkedList(val, p->next);
        _size++;
    }
    
    void deleteAtIndex(int index) {
        if (index >= _size) {
            return;
        }
        LinkedList *p = _dummyHead;
        for (int i = 0; i < index; i++) {
            p = p->next;
        }
        LinkedList *temp = p->next;
        p->next = temp->next;
        delete temp;
        _size--;
    }

private: // 声明成员变量
    int _size;
    LinkedList *_dummyHead;
};

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList* obj = new MyLinkedList();
 * int param_1 = obj->get(index);
 * obj->addAtHead(val);
 * obj->addAtTail(val);
 * obj->addAtIndex(index,val);
 * obj->deleteAtIndex(index);
 */
```

---

## 双链表

注意多处理三个前置节点。

```cpp
class MyLinkedList {
public:
    struct LinkNode {
        int val;
        LinkNode *prev;
        LinkNode *next;
        LinkNode(int val, LinkNode *prev, LinkNode *next) : val(val), prev(prev), next(next) {}
        LinkNode(int val) : val(val), prev(nullptr), next(nullptr) {}
        LinkNode() : val(0), prev(nullptr), next(nullptr) {}
    };

    MyLinkedList() {
        _dummyHead = new LinkNode(0);
        _size = 0;
    }
    
    int get(int index) {
        if (index >= _size) {
            return -1;
        }

        LinkNode *p = _dummyHead->next;
        for (int i = 0; i < index; i++) {
            p = p->next;
        }

        return p->val;
    }
    
    void addAtHead(int val) {
        _dummyHead->next = new LinkNode(val, _dummyHead, _dummyHead->next);
        if (_dummyHead->next->next != nullptr) { // 处理前置节点
            _dummyHead->next->next->prev = _dummyHead->next;
        }
        _size++;
    }
    
    void addAtTail(int val) {
        LinkNode *p = _dummyHead;
        for (int i = 0; i < _size; i++) {
            p = p->next;
        }

        p->next = new LinkNode(val, p, nullptr);
        _size++;
    }
    
    void addAtIndex(int index, int val) {
        if (index == 0) {
            addAtHead(val);
            return;
        }
        if (index == _size) {
            addAtTail(val);
            return;
        }
        if (index > _size) {
            return;
        }

        LinkNode *p = _dummyHead;
        for (int i = 0; i < index; i++) {
            p = p->next;
        }
        p->next = new LinkNode(val, p, p->next);
        if (p->next->next != nullptr) { // 处理前置节点
            p->next->next->prev = p->next;
        }
        _size++;
    }
    
    void deleteAtIndex(int index) {
        if (index >= _size) {
            return;
        }

        LinkNode *p = _dummyHead;
        for (int i = 0; i < index; i++) {
            p = p->next;
        }

        LinkNode *temp = p->next;
        p->next = temp->next;
        if (p->next != nullptr) { // 处理前置节点
            p->next->prev = p;
        }
        delete temp; 
        _size--;
    }

private:
    int _size;
    LinkNode *_dummyHead;
};

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList* obj = new MyLinkedList();
 * int param_1 = obj->get(index);
 * obj->addAtHead(val);
 * obj->addAtTail(val);
 * obj->addAtIndex(index,val);
 * obj->deleteAtIndex(index);
 */
```

---
