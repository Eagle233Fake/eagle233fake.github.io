---
title: '[Algorithms] LeetCode 454. 四数相加 II'
date: 2025-04-09 16:41:25
tags:
categories: 
- Algorithms
- HashTable
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 454. 四数相加 II](https://leetcode.cn/problems/4sum-ii/)

## `unordered_map`

`key`存和，`value`存出现次数。

```cpp
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        unordered_map<int, int> umap;
        for (int a : nums1) {
            for (int b : nums2) {
                umap[a + b]++;
            }
        }

        int count = 0;

        for (int c : nums3) {
            for (int d : nums4) {
                if (umap.find(-(c + d)) != umap.end()) {
                    count += umap[-(c + d)]; // 注意增加的是umap中的次数
                }
            }
        }

        return count;
    }
};
```

三刷：这样做为什么不对呢？

```cpp
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        unordered_set<int> uset1;
        int n = nums1.size();
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                uset1.insert(nums1[i] + nums2[j]);
            }
        }

        int r = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                auto iter = uset1.find(-(nums3[i] + nums4[j]));
                if (iter != uset1.end()) {
                    r++;
                }
            }
        }

        return r;
    }
};
```

因为实际上每个`set`中的元素实际上可能出现了多次，这样做都变成一次了。

---
