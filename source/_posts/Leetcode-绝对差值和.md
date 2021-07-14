---
title: Leetcode-绝对差值和
date: 2021-07-14 14:13:00
categories: 刷题记录
tags: 刷题
---

## 题目描述

两个正整数数组 nums1 和 nums2 ，数组的长度都是 n 。

数组 nums1 和 nums2 的绝对差值和定义为所有 |nums1[i] - nums2[i]|（0 <= i < n）的总和（下标从 0 开始）。

可以选用 nums1 中的任意一个元素来替换 nums1 中的至多一个元素，以最小化绝对差值和。

在替换数组 nums1 中最多一个元素之后 ，返回最小绝对差值和。因为答案可能很大，所以需要对 10^9 + 7 取余后返回。

**刷题链接**：[https://leetcode-cn.com/problems/minimum-absolute-sum-difference](https://leetcode-cn.com/problems/minimum-absolute-sum-difference)

<!--more-->

## 解法

```C++
int minAbsoluteSumDiff(vector<int>& nums1, vector<int>& nums2) {
    const int mod = 1e9 + 7;
    vector<int> tmp(nums1);
    int n = nums1.size(), maxVal = 0;
    int res = 0;
    sort(tmp.begin(), tmp.end());
    for (int i = 0; i < n; i++) {
        int diff = abs(nums1[i] - nums2[i]);
        res = (res + diff) % mod;
        int index = lower_bound(tmp.begin(), tmp.end(), nums2[i]) - tmp.begin(); // 二分查找
        if (index < n) {
            maxVal = max(maxVal, diff - (tmp[index] - nums2[i])); // 与第一个大于等于的数之差
        }
        if (index > 0) {
            maxVal = max(maxVal, diff - (nums2[i] - tmp[index - 1])); // 与第一个小于的数之差
        }
    }
    return (res - maxVal + mod) % mod;
}
```
