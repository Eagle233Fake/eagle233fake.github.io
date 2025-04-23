---
title: '[Algorithms] LeetCode 1. 两数之和'
date: 2025-04-09 16:40:36
tags:
categories: 
- Algorithms
- HashTable
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 1. 两数之和](https://leetcode.cn/problems/two-sum/)

## 迭代器用法

要保存下标，所以用`map`。注意迭代器用法。

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> map;
        for (int i = 0; i < nums.size(); i++) {
            auto iter = map.find(target - nums[i]);
            if (iter != map.end()) {
                return {i, iter->second};
            }
            map.insert({nums[i], i});
        }
        return {};
    }
};
```

---
