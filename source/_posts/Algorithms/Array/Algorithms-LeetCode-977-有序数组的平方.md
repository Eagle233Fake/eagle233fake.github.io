---
title: "[Algorithms] LeetCode 977. 有序数组的平方"
date: 2025-04-07 16:53:02
tags:
categories:
- Algorithms
- Array
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 977. 有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array)

双指针法，倒序写进结果数组。

四刷：指针可以取等，为什么？

```cpp
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        int i = 0;
        int j = nums.size() - 1;
        vector<int> result(nums.size());
        int k = nums.size() - 1;
        while (i <= j) { // 可以取等
            if (nums[i] * nums[i] < nums[j] * nums[j]) {
                result[k--] = nums[j] * nums[j];
                j--;
            } else {
                result[k--] = nums[i] * nums[i];
                i++;
            }
        }

        return result;
    }
};
```

---
