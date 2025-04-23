---
title: '[Algorithms] LeetCode 18. 四数之和'
date: 2025-04-10 13:30:53
tags:
categories: 
- Algorithms
- HashTable
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 18. 四数之和](https://leetcode.cn/problems/4sum/)

## 双指针

两层`for`循环，两个剪枝，四个去重。加法数据开`long long`。

```cpp
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());

        for (int i = 0; i < nums.size(); i++) {
            if (target > 0 && nums[i] > target) {
                break;
            }
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            for (int j = i + 1; j < nums.size(); j++) {
                if (target > 0 && nums[i] + nums[j] > target) {
                    break;
                }
                if (j > i + 1 && nums[j] == nums[j - 1]) {
                    continue;
                }

                int l = j + 1, r = nums.size() - 1;

                while (l < r) {
                    if ((long long)nums[i] + nums[j] + nums[l] + nums[r] < target) {
                        l++;
                    } else if ((long long)nums[i] + nums[j] + nums[l] + nums[r] > target) {
                        r--;
                    } else {
                        result.push_back({nums[i], nums[j], nums[l], nums[r]});
                        while (l < r && nums[l] == nums[l + 1]) {
                            l++;
                        }
                        while (l < r && nums[r] == nums[r - 1]) {
                            r--;
                        }
                        l++;
                        r--;
                    }
                }
            }
        }
        return result;
    }
};
```

---
