---
title: '[Algorithms] LeetCode 225. 用队列实现栈'
date: 2025-04-21 16:04:28
tags: 
categories:
- Algorithms
- StackAndQueue
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 225. 用队列实现栈](https://leetcode.cn/problems/implement-stack-using-queues/)

## 单个队列模拟栈

```cpp
class MyStack {
public:

    queue<int> que;

    MyStack() {
        
    }
    
    void push(int x) {
        que.push(x);
    }
    
    int pop() {
        int size = que.size() - 1;
        while (size--) {
            que.push(que.front());
            que.pop();
        }

        int a = que.front();
        que.pop();
        return a;
    }
    
    int top() {
        int a = this->pop();
        this->push(a);
        return a;
    }
    
    bool empty() {
        return que.empty();
    }
};

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack* obj = new MyStack();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->top();
 * bool param_4 = obj->empty();
 */
```

---
