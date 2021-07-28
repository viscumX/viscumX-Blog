---
title: Leetcode-二叉树中第二小的节点
date: 2021-07-28 11:40:57
categories: 刷题记录
tags: 刷题
---

## 题目描述

给定一个非空特殊的二叉树，每个节点都是正数，并且每个节点的子节点数量只能为  2  或  0。如果一个节点有两个子节点的话，那么该节点的值等于两个子节点中较小的一个。root.val = min(root.left.val, root.right.val) 总成立。

给出这样的一个二叉树，输出所有节点中的第二小的值。如果第二小的值不存在的话，输出 -1 。

**刷题链接**：[https://leetcode-cn.com/problems/second-minimum-node-in-a-binary-tree](https://leetcode-cn.com/problems/second-minimum-node-in-a-binary-tree)

<!--more-->

## 解法

树中等于`root->val`的都是最小节点

```C++
int res = -1;
void helper(TreeNode* root, int cur) {
    if (!root) {
        return;
    }
    if (root->val != cur) {
        // 还没有出现过第二小的
        if (res == -1) {
            res = root->val;
        } else { // 出现过了
            res = min(res, root->val);
        }
        return;
    }
    helper(root->left, cur);
    helper(root->right, cur);
}
int findSecondMinimumValue(TreeNode* root) {
    if (!root) {
        return -1;
    }
    helper(root, root->val);
    return res;
}
```
