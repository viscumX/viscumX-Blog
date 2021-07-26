---
title: Leetcode-组合
date: 2021-07-21 11:58:16
categories: 刷题记录
tags: 刷题
---

## 题目描述

给定两个整数 n 和 k，返回范围 [1, n] 中所有可能的 k 个数的组合。可以按任何顺序返回答案。

**刷题链接**：[https://leetcode-cn.com/problems/combinations/](https://leetcode-cn.com/problems/combinations/)

<!--more-->

## 解法

经典回溯

```C++
vector<vector<int>> res;
vector<int> path;
void helper(int n, int k, int index) {
    if (path.size() == k) {
        res.push_back(path);
        return;
    }
    for (int i = index; i <= n; i++) {
        path.push_back(i);
        helper(n, k, i + 1);
        path.pop_back();
    }
}
vector<vector<int>> combine(int n, int k) {
    helper(n, k, 1);
    return res;
}
```

回溯模板，参考[https://www.cnblogs.com/steven_oyj/archive/2010/05/22/1741376.html](https://www.cnblogs.com/steven_oyj/archive/2010/05/22/1741376.html)

```C++
int a[n];
try(int i)
{
    if (i > n)
        // 输出结果;
    else
    {
        for (j = 下界; j <= 上界; j = j + 1) // 枚举i所有可能的路径
        {
            if (fun(j)) // 满足限界函数和约束条件
            {
                a[i] = j;
                // 其他操作
                try(i + 1);
                // 回溯前的清理工作(如a[i] 置空值等);
            }
        }
    }
}
```
