---
title: Leetcode-接雨水
date: 2021-07-20 19:00:20
categories: 刷题记录
tags: 刷题
---

## 题目描述

给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

**刷题链接**：[https://leetcode-cn.com/problems/trapping-rain-water/](https://leetcode-cn.com/problems/trapping-rain-water/)

<!--more-->

## 解法

### 动态规划

位置 i 的水位等于两边最大高度的最小值减去当前高度。使用动态规划可以通过一次扫描得到一边的最大高度

```C++
int trap(vector<int> &height)
{
    int n = height.size();
    if (n == 0)
    {
        return 0;
    }
    vector<int> leftMax(n);
    leftMax[0] = height[0];
    for (int i = 1; i < n; i++)
    {
        leftMax[i] = max(leftMax[i - 1], height[i]);
    }
    vector<int> rightMax(n);
    rightMax[n - 1] = height[n - 1];
    for (int i = n - 2; i >= 0; i--)
    {
        rightMax[i] = max(rightMax[i + 1], height[i]);
    }
    int res = 0;
    for (int i = 0; i < n; i++)
    {
        res += min(leftMax[i], rightMax[i]) - height[i];
    }
    return res;
}
```

### 单调栈

```C++
int trap(vector<int> &height)
{
    int res = 0;
    int n = height.size();
    stack<int> s;
    for (int i = 0; i < n; i++)
    {
        while (!s.empty() && height[i] > height[s.top()])
        {
            int t = s.top();
            s.pop();
            if (s.empty())
            {
                break;
            }
            int left = s.top();
            int curWidth = i - left - 1;
            int curHeight = min(height[left], height[i]) - height[t];
            res += curWidth * curHeight;
        }
        s.push(i);
    }
    return res;
}
```

### 双指针

```C++
int trap(vector<int> &height)
{
    int res = 0;
    int left = 0, right = height.size() - 1;
    int leftMax = 0, rightMax = 0;
    while (left < right)
    {
        leftMax = max(leftMax, height[left]);
        rightMax = max(rightMax, height[right]);
        if (height[left] < height[right])
        {
            res += leftMax - height[left];
            left++;
        }
        else
        {
            res += rightMax - height[right];
            right--;
        }
    }
    return res;
}
```
