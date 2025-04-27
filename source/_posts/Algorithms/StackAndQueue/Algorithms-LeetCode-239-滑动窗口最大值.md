---
title: '[Algorithms] LeetCode 239. 滑动窗口最大值'
date: 2025-04-22 13:39:33
tags:
categories:
- Algorithms
- StackAndQueue
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 239. 滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/)

## 自定义单调队列

注意检查非空；注意第一次要加入最大值；注意单调队列如何维持；注意弹出时要判断是否需要弹出。

```cpp
class Solution {
private:
    class myQueue {
    public:
        deque<int> que;

        void pop(int n) {
            if (!que.empty() && que.front() == n) {
                que.pop_front();
            }
        }

        void push(int n) {
            if (!que.empty() && n <= que.back()) {
                que.push_back(n);
            } else {
                while (!que.empty() && n > que.back()) {
                    que.pop_back(); // 右边比较小的全部pop掉
                }
                que.push_back(n);
            }
        }

        int front(void) {
            return que.front();
        }
    };

public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        myQueue q;
        for (int i = 0; i < k; i++) {
            q.push(nums[i]);
        }

        int i = 0, j = k - 1;
        vector<int> v;
        v.push_back(q.front()); // 第一次的结果要记录
        for (int i = k; i < nums.size(); i++) {
            q.pop(nums[i - k]);
            q.push(nums[i]);
            v.push_back(q.front());
        }

        return v;
    }
};
```

---
