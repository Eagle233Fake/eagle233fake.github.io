---
title: '[Algorithms] LeetCode 15. 三数之和'
date: 2025-04-10 13:29:01
tags:
categories: 
- Algorithms
- HashTable
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 15. 三数之和](https://leetcode.cn/problems/3sum/)

## 双指针

上一道三数之和不需要元素查重，但是本题使用`map`不容易查重，因此使用双指针。可以进行一次剪枝和三次查重。

三刷：双指针内部不要用`while`，用`if`方便判断逻辑。

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> result;
        for (int i = 0; i < nums.size() - 2; i++) {
            if (nums[i] > 0) { // 剪枝
                break;
            }
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue; // 第一次去重
            }
            int left = i + 1;
            int right = nums.size() - 1;
            while (left < right) {
                if (nums[left] + nums[right] + nums[i] < 0) {
                    left++;
                } else if (nums[left] + nums[right] + nums[i] > 0) {
                    right--;
                } else {
                    result.push_back({nums[i], nums[left], nums[right]});
                    while (left < right && nums[left] == nums[left + 1]) { // 第二次
                        left++;
                    }
                    while (left < right && nums[right] == nums[right - 1]) { // 第三次
                        right--;
                    }
                    left++;
                    right--;
                }
            }
        }

        return result;
    }
};
```

---
