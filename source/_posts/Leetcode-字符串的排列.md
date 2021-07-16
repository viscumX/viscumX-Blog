---
title: Leetcode-字符串的排列
date: 2021-07-16 19:49:51
categories: 刷题记录
tags: 刷题
---

## 题目描述

给定两个字符串 s1 和 s2，写一个函数来判断 s1 的排列之一是否是 s2 的子串。

**刷题连接**：[https://leetcode-cn.com/problems/permutation-in-string/](https://leetcode-cn.com/problems/permutation-in-string/)

<!--more-->

## 解法

### 双指针

用一个长度为 26 的数组记录 s1 中每个字母出现的频数，对于 s2 在`[left, right]`内的数 x，如果是 s1 的排列中的一部分，应该它的频数应该`>=0`，假如`[left, right]`的区间长度能达到 s1 的长度，就说明存在

```C++
bool checkInclusion(string s1, string s2) {
    int n1 = s1.length(), n2 = s2.length();
    vector<int> cnt(26);
    // 记录每个字母出现的频数
    for (auto c : s1) {
        cnt[c - 'a']++;
    }
    int left = 0;
    for (int right = 0; right < n2; right++) {
        int x = s2[right] - 'a';
        cnt[x]--;
        // 假设频数超过了 s1，就移动 left 指针
        while (cnt[x] < 0) {
            // 还原 cnt
            cnt[s2[left] - 'a']++;
            left++;
        }
        if (right - left + 1 == n1) {
            return true;
        }
    }
    return false;
}
```

### 滑动窗口

用一个长度为 n 的窗口在 s2 中检查字母出现的次数，直到与 s1 中完全一致

```Go
func checkInclusion(s1 string, s2 string) bool {
    n1, n2 := len(s1), len(s2)
    if n1 > n2 {
        return false
    }
    var cnt1, cnt2 [26]int
    // 分别记录 cnt1 和 cnt2 中的字母出现字数
    for i, c := range s1 {
        cnt1[c - 'a']++
        cnt2[s2[i] - 'a']++
    }
    // Go 的数组可以直接比较
    if cnt1 == cnt2 {
        return true
    }
    for i := n1; i < n2; i++ {
        // 进出滑动窗口
        cnt2[s2[i] - 'a']++
        cnt2[s2[i - n1] - 'a']--
        if cnt1 == cnt2 {
            return true
        }
    }
    return false
}
```
