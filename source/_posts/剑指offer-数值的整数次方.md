---
title: 剑指offer-数值的整数次方
date: 2021-07-26 14:37:44
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

实现函数 double Power(double base, int exponent)，求 base 的 exponent 次方。

不得使用库函数，同时不需要考虑大数问题。

只要输出结果与答案的绝对误差不超过 10−2 即视为正确。

**刷题链接**：[https://leetcode-cn.com/problems/powx-n/](https://leetcode-cn.com/problems/powx-n/)

<!--more-->

## 解法

### 快速幂 + 递归

```C++
double quickMul(double x, long long n) {
    if (n == 0) {
        return 1.0;
    }
    double y = quickMul(x, n / 2);
    return n % 2 == 0 ? y * y : y * y * x;
}
double myPow(double x, int n) {
    long long expo = n;
    return expo >= 0 ? quickMul(x, expo) : 1.0 / quickMul(x, -expo);
}
```

### 快速幂 + 迭代

将指数转为二进制，有 1 就说明有贡献

```C++
    double quickMul(double x, long long n) {
        double res = 1.0;
        while (n > 0) {
            if (n % 2)
                res *= x;
            x *= x;
            n /= 2;
        }
        return res;
    }
    double Power(double base, int exponent) {
        long long n = exponent;
        return n >= 0 ? quickMul(base, n) : 1.0 / quickMul(base, -n);
    }
```

避免使用递归栈，将空间复杂度从$O(logN)$降到$O(1)$
