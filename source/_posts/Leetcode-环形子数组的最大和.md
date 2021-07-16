---
title: Leetcode-环形子数组的最大和
date: 2021-07-16 14:59:51
categories: 刷题记录
tags: 刷题
---

## 题目描述

给定一个由整数数组 A  表示的环形数组 C，求 C  的非空子数组的最大可能和。

在此处，环形数组意味着数组的末端将会与开头相连呈环状。（形式上，当 0 <= i < A.length  时  C[i] = A[i]，且当  i >= 0  时  C[i+A.length] = C[i]）

此外，子数组最多只能包含固定缓冲区 A  中的每个元素一次。（形式上，对于子数组  C[i], C[i+1], ..., C[j]，不存在  i <= k1, k2 <= j  其中  k1 % A.length = k2 % A.length）

**刷题链接**：[https://leetcode-cn.com/problems/maximum-sum-circular-subarray](https://leetcode-cn.com/problems/maximum-sum-circular-subarray)

<!--more-->

## 解法

非分段的最大子数组和就是普通数组的最大子序和，分段的最大子数组和就是数组总和减去最小子序和

```C++
int maxSubarraySumCircular(vector<int>& nums) {
    int sum = 0, maxSum = INT_MIN, minSum = INT_MAX;
    int curMax = 0, curMin = 0;
    for (int num: nums) {
        sum += num;
        curMax = num + max(0, curMax);
        curMin = num + min(0, curMin);
        maxSum = max(curMax, maxSum);
        minSum = min(curMin, minSum);
    }
    if (sum == minSum) {    // 假如相等，说明没有正数，就返回最大子序和
        return maxSum;
    } else {
        return max(maxSum, sum - minSum);
    }
}
```
