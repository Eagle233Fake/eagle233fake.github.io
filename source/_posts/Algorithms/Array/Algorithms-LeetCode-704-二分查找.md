---
title: "[Algorithms] LeetCode 704. 二分查找"
date: 2025-04-07 16:30:26
tags:
categories: 
- Algorithms
- Array
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 704. 二分查找](https://leetcode.cn/problems/binary-search/)

注意区间的写法，我这里是**左闭右开**。CPP中很多STL容器都是左闭右开的，统一一下写法。

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size();
        while (left < right) {
            int middle = left + (right - left) / 2;
            if (nums[middle] == target) {
                return middle;
            } else if (nums[middle] < target) {
                left = middle + 1;
            } else {
                right = middle;
            }
        }

        return -1;
    }
};
```

---
