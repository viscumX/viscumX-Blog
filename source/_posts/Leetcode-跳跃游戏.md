---
title: Leetcode-跳跃游戏
date: 2021-07-15 17:12:17
categories: 刷题记录
tags: 刷题
---

## 题目描述

给定一个非负整数数组  nums ，最初位于数组的 第一个下标 。

数组中的每个元素代表可以跳跃的最大长度。

判断是否能够到达最后一个下标。

**刷题链接**：[https://leetcode-cn.com/problems/jump-game](https://leetcode-cn.com/problems/jump-game)

<!--more-->

## 解法

贪心做法，假如能跳跃到的最远距离大于当前下标，就说明能到达当前点，并更新最远距离

```C++
bool canJump(vector<int>& nums) {
    int maxn = 0;
    for (int i = 0; i < nums.size(); i++) {
        if (maxn < i) { //假如跳不到当前下标
            return false;
        }
        maxn = max(maxn, i + nums[i]);
    }
    return true;
}
```
