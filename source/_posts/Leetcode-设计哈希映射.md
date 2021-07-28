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

一维数组实现

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

### 链表法

用链表来节省空间

```C++
class MyHashMap {
public:
    /** Initialize your data structure here. */
    MyHashMap() {
        size = 1009;
        hashmap.resize(size);
    }

    /** value will always be non-negative. */
    void put(int key, int value) {
        Node *pNode = new Node(key, value);
        int idx = getIndex(key);
        if (!hashmap[idx]) { // 位置无key
            hashmap[idx] = pNode;
        } else {
            Node *cur = hashmap[idx];
            while (cur && cur->key != key) {
                cur = cur->next;
            }
            if (!cur) { // 之前位置有其他key
                pNode->next = hashmap[idx];
                hashmap[idx] = pNode;
            } else { // 之前有值
                cur->val = value;
            }
        }
    }

    /** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    int get(int key) {
        int idx = getIndex(key);
        if (!hashmap[idx]) {
            return -1;
        }
        Node *cur = hashmap[idx];
        while (cur && cur->key != key) {
            cur = cur->next;
        }
        return cur == nullptr ? -1 : cur->val;
    }

    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    void remove(int key) {
        if (get(key) == -1) {
            return;
        }
        int idx = getIndex(key);
        Node *cur = hashmap[idx];
        if (cur->key == key) {
            Node *delNode = hashmap[idx]->next;
            cur->next = cur;
            hashmap[idx] = delNode;
        } else {
            while (cur && cur->next && cur->next->key != key) {
                cur = cur->next;
            }
            Node *delNode = cur->next;
            cur->next = delNode->next;
            delNode->next = delNode;
        }
    }
private:
    struct Node {
        int key, val;
        Node *next;
        Node(int _key, int _val) : key(_key), val(_val), next(nullptr) { }
    };
    vector<Node*> hashmap;
    int size;
    int getIndex(int x) {
        return x % size;
    }
};
```

还可以用 list 取代链表，实现上更加简单

```C++
class MyHashMap {
public:
    /** Initialize your data structure here. */
    MyHashMap() : data(base) {

    }

    /** value will always be non-negative. */
    void put(int key, int value) {
        int h = hash(key);
        for (auto it = data[h].begin(); it != data[h].end(); ++it) {
            if (it->first == key) {
                it->second = value;
                return;
            }
        }
        data[h].push_back(make_pair(key, value));
    }

    /** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    int get(int key) {
        int h = hash(key);
        for (auto it = data[h].begin(); it != data[h].end(); ++it){
            if (it->first == key) {
                return it->second;
            }
        }
        return -1;
    }

    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    void remove(int key) {
        int h = hash(key);
        for (auto it = data[h].begin(); it != data[h].end(); ++it) {
            if (it->first == key) {
                data[h].erase(it);
                return;
            }
        }
    }
private:
    vector<list<pair<int, int>>> data;
    static const int base = 769;
    int hash(int key) {
        return key % base;
    }
};
```

### 开放寻址法

同时记录 key 和 value，并进行地址的线性探测，直至表填满

```C++
class MyHashMap {
public:
    /** Initialize your data structure here. */
    MyHashMap() {
        hashmap = vector<pair<int, int>>(N, {-1, -1});
    }

    /** value will always be non-negative. */
    void put(int key, int value) {
        int k = hash(key);
        hashmap[k] = {key, value};
    }

    /** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    int get(int key) {
        int k = hash(key);
        if (hashmap[k].first == -1) {
            return -1;
        }
        return hashmap[k].second;
    }

    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    void remove(int key) {
        int k = hash(key);
        if (hashmap[k].first != -1) {
            hashmap[k].first = -2;
        }
    }
private:
    const static int N = 20011;
    vector<pair<int, int>> hashmap;
    int hash(int key) {
        int k = key % N;
        while (hashmap[k].first != key && hashmap[k].first != -1) {
            k = (k + 1) % N;
        }
        return k;
    }
};
```
