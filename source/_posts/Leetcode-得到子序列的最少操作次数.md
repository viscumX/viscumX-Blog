---
title: Leetcode-得到子序列的最少操作次数
date: 2021-07-26 11:03:46
categories: 刷题记录
tags: 刷题
---

## 题目描述

一个数组 target，包含若干互不相同的整数，以及另一个整数数组 arr，arr 可能包含重复元素。

每一次操作中，可以在 arr 的任意位置插入任一整数。比方说，如果 arr=[1,4,1,2]，那么可以在中间添加 3 得到[1,4,3,1,2] 。可以在数组最开始或最后面添加整数。

返回最少操作次数，使得 target 成为 arr 的一个子序列。

一个数组的子序列指的是删除原数组的某些元素（可能一个元素都不删除），同时不改变其余元素的相对顺序得到的数组。比方说，[2,7,4]是[4,2,3,7,2,1,4]的子序列，但[2,4,2]不是。

**刷题链接**：[https://leetcode-cn.com/problems/minimum-operations-to-make-a-subsequence](https://leetcode-cn.com/problems/minimum-operations-to-make-a-subsequence)

<!--more-->

## 解法

将 arr 转化为当前元素在 target 中的下标，只要找到其中最长的递增子序列就行。

对于最长递增子序列问题，参考[https://leetcode-cn.com/problems/longest-increasing-subsequence/](https://leetcode-cn.com/problems/longest-increasing-subsequence/)，有 dp 和贪心+二分查找两种解法

```C++
int minOperations(vector<int>& target, vector<int>& arr) {
    int n = target.size();
    unordered_map<int, int> pos;
    for (int i = 0; i < n; ++i) {
        pos[target[i]] = i;
    }
    vector<int> d;
    for (int val : arr) {
        if (pos.count(val)) {
            int idx = pos[val];
            auto it = lower_bound(d.begin(), d.end(), idx);
            if (it != d.end()) {
                *it = idx;
            } else {
                d.push_back(idx);
            }
        }
    }
    return n - d.size();
}
```
