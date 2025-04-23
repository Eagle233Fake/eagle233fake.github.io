---
title: '[Algorithms] LeetCode 347. 前 K 个高频元素'
date: 2025-04-22 13:39:41
tags:
categories:
- Algorithms
- StackAndQueue
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 347. 前 K 个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/description/)

## 优先队列 小顶堆

重载的操作必须是公开的；元素、底层容器、比较函数被定义在优先队列中；输出要倒序。

```cpp
class Solution {
public:
    class myComparison {
    public: // 公开
        bool operator()(const pair<int, int> &lhs, const pair<int, int> &rhs) {
            return lhs.second > rhs.second; // 小顶堆 与直觉相反
        }
    };
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> umap;
        for (auto i : nums) {
            umap[i]++;
        }

        priority_queue<pair<int, int>, vector<pair<int, int>>, myComparison> pq;
        for (auto it : umap) {
            pq.push(it);
            if (pq.size() > k) {
                pq.pop();
            }
        }

        vector<int> result;
        for (int i = pq.size() - 1; i >= 0 ; i--) {
            result.push_back(pq.top().first);
            pq.pop();
        }

        return result;
    }
};
```

---
