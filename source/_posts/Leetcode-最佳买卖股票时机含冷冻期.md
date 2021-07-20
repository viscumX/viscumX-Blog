---
title: Leetcode-最佳买卖股票时机含冷冻期
date: 2021-07-20 11:27:17
categories: 刷题记录
tags: 刷题
---

## 题目描述

给定一个整数数组，其中第 i 个元素代表了第 i 天的股票价格。
​
计算出最大利润。在满足以下约束条件下，可以尽可能地完成更多的交易（多次买卖一支股票）:

- 不能同时参与多笔交易（必须在再次购买前出售掉之前的股票）。
- 卖出股票后，无法在第二天买入股票 (即冷冻期为 1 天)。

**刷题链接**：[https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown)

<!--more-->

## 解法

每天都可以处于三种状态：

- 持有股票
- 不持有股票，但处于冷冻期
- 不持有股票，也不处于冷冻期

最后一天要最大收益应该就后两种状态

```C++
int maxProfit(vector<int>& prices) {
    int dp0 = -prices[0], dp1 = 0, dp2 = 0;
    for (int i = 1; i < prices.size(); i++) {
        // 持有股票的最大收益可能是前一天也持有的，也有可能是前一天处于冷冻期，今天刚买
        int tmp0 = max(dp0, dp2 - prices[i]);
        // 前一天卖出股票
        int tmp1 = dp0 + prices[i];
        // 前一天处于冷冻期或不持有股票也不处于冷冻期
        int tmp2 = max(dp1, dp2);
        dp0 = tmp0, dp1 = tmp1, dp2 = tmp2;
    }
    return max(dp1, dp2);
}
```
