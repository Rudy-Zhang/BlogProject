title: "leetcode习题集 104 Maximum Depth of Binary Tree  "
date: 2015-04-06 15:31:44
category: [leetcode习题集]
tags: []
---
Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

题意：给定一个二叉树，找出最大深度

思路：
使用递归解法




```
class Solution {
public:
//使用递归
//左右只有一个子树不影响
    int maxDepth(TreeNode *root) {
        if(root==NULL) return 0;
        int left=maxDepth(root->left)+1;
        int right=maxDepth(root->right)+1;
        return (left>=right?left:right);
    }
};
```
