---
title: '[Algorithms] LeetCode 349. 两个数组的交集'
date: 2025-04-09 16:39:05
tags:
categories: 
- Algorithms
- HashTable
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 349. 两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays/)

## `unordered_set`

考察`unordered_set`用法。注意插入操作、查询操作、转换为别的容器的做法。

```cpp
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> us;
        unordered_set<int> result;
        for (int i = 0; i < nums1.size(); i++) {
            us.insert(nums1[i]);
        }

        for (int i = 0; i < nums2.size(); i++) {
            if (us.find(nums2[i]) != us.end()) {
                result.insert(nums2[i]);
            }
        }

        return vector<int>(result.begin(), result.end());
    }
};
```

---
