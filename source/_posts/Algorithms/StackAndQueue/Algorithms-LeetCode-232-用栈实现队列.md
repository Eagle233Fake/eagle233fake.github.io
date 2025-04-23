---
title: '[Algorithms] LeetCode 232.用栈实现队列'
date: 2025-04-17 21:02:25
tags: 
categories:
- Algorithms
- StackAndQueue
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 232.用栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/)

## 直观的方法

效率不高。

```cpp
class MyQueue {
public:
    stack<int> in;
    stack<int> out;

    MyQueue() {
        
    }
    
    void push(int x) {
        in.push(x);
    }
    
    int pop() {
        while (!in.empty()) {
            out.push(in.top());
            in.pop();
        }
        int n = out.top();
        out.pop();
        while (!out.empty()) {
            in.push(out.top());
            out.pop();
        }
        return n;
    }
    
    int peek() {
        while (!in.empty()) {
            out.push(in.top());
            in.pop();
        }
        int n = out.top();
        while (!out.empty()) {
            in.push(out.top());
            out.pop();
        }
        return n;
    }
    
    bool empty() {
        if (in.empty()) {
            return true;
        }
        return false;
    }
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue* obj = new MyQueue();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->peek();
 * bool param_4 = obj->empty();
 */
```

---

## 复用

在`pop()`中，为什么要判断`out`是空的？

```cpp
class MyQueue {
public:
    stack<int> in;
    stack<int> out;

    MyQueue() {
        
    }
    
    void push(int x) {
        in.push(x);
    }
    
    int pop() {
        if (out.empty()) {
            while (!in.empty()) {
                out.push(in.top());
                in.pop();
            }
        }
        
        int a = out.top();
        out.pop();
        return a;
    }
    
    int peek() {
        int a = this->pop();
        out.push(a);
        return a;
    }
    
    bool empty() {
        if (out.empty() && in.empty()) {
            return true;
        }
        return false;
    }
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue* obj = new MyQueue();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->peek();
 * bool param_4 = obj->empty();
 */
```

---
