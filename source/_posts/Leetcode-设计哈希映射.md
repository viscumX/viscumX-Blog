---
title: Leetcode-设计哈希映射
date: 2021-07-26 16:55:26
categories: 刷题记录
tags: 刷题
---

## 题目描述

不使用任何内建的哈希表库设计一个哈希映射（HashMap）

实现 MyHashMap 类：

- MyHashMap() 用空映射初始化对象
- void put(int key, int value) 向 HashMap 插入一个键值对 (key, value) 。如果 key 已经存在于映射中，则更新其对应的值 value
- int get(int key) 返回特定的 key 所映射的 value ；如果映射中不包含 key 的映射，返回 -1
- void remove(key) 如果映射中存在 key 的映射，则移除 key 和它所对应的 value

**刷题链接**：[https://leetcode-cn.com/problems/design-hashmap](https://leetcode-cn.com/problems/design-hashmap)

<!--more-->

## 解法

### 简单数组

```C++
class MyHashMap {
public:
    /** Initialize your data structure here. */
    vector<int> h;
    MyHashMap() {
        const int N = 1000009;
        h = vector<int> (N, -1);
    }

    /** value will always be non-negative. */
    void put(int key, int value) {
        h[key] = value;
    }

    /** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    int get(int key) {
        return h[key];
    }

    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    void remove(int key) {
        h[key] = -1;
    }
};
```

###
