---
title: Leetcode-乘积最大子数组
date: 2021-07-18 10:39:05
categories: 刷题记录
tags: 刷题
---

## 题目描述

一个整数数组 nums ，请找出数组中乘积最大的连续子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。

**刷题链接**：[https://leetcode-cn.com/problems/maximum-product-subarray/](https://leetcode-cn.com/problems/maximum-product-subarray/)

<!--more-->

## 解法

分别记录最大数和最小数

```C++
int maxProduct(vector<int>& nums) {
    int n = nums.size();
    int res = nums[0], maxP = nums[0], minP = nums[0];
    for (int i = 1; i < n; i++) {
        int tmp1 = maxP, tmp2 = minP;
        maxP = max(tmp1 * nums[i], max(nums[i], tmp2 * nums[i]));
        minP = min(tmp2 * nums[i], min(nums[i], tmp1 * nums[i]));
        res = max(maxP, res);
    }
    return res;
}
```
