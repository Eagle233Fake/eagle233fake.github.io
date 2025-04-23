---
title: "[Algorithms] LeetCode 27. 移除元素"
date: 2025-04-07 16:41:23
tags:
categories: 
- Algorithms
- Array
---

> 来源：[代码随想录](https://programmercarl.com/)

[LeetCode 27. 移除元素](https://leetcode.cn/problems/remove-element)

## 暴力解法

这边先要初始化一个`size`变量，原因是我们需要一直变动`size`的大小，不希望更改实际`size`之外的数组部分。

```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int i = 0;
        int size = nums.size(); // 防止size一直变更
        while (i != size) {
            if (nums[i] == val) {
                for (int j = i; j < size - 1; j++) {
                    nums[j] = nums[j + 1];
                }
                size--;
            } else {
                i++;
            }
        }

        return size;
    }
};
```

---

## 双指针

```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int slow = 0;
        int fast = 0;
        while (fast < nums.size()) {
            if (nums[fast] == val) {
                fast++;
            } else {
                nums[slow] = nums[fast];
                slow++;
                fast++;
            }
        }
        return slow;
    }
};
```

---
