---
title: "[Algorithms] LeetCode 209. 长度最小的子数组"
date: 2025-04-07 18:13:16
tags:
categories: 
- Algorithms
- Array
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 209. 长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/)

滑动窗口，注意指针什么时候要++。指针最好不要定义在`for`循环里面，有时候要回去--，很乱，不如直接手动++。

三刷发现忘记左边指针要是`while`循环了，如果不满足左指针要一直动，引起重视。

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int ssum = 0;
        for (int i = 0; i < nums.size(); i++) {
            ssum += nums[i];
        }
        if (ssum < target) {
            return 0;
        }
        
        int left = 0, right = 0;
        int sum = 0;
        int minLen = INT_MAX;
        while (right < nums.size()) {
            sum += nums[right];
            // right++; 在这里++就会导致结果差1
            while (sum >= target) {
                minLen = min(minLen, right - left + 1);
                sum -= nums[left];
                left++;
            }
            right++;
        }

        return minLen;
    }
};
```

---
