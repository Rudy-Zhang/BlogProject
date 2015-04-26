title: "leetcode习题集 111 Minimum Depth of Binary Tree"
date: 2015-04-06 15:31:44
category: [leetcode习题集]
tags: []
---
Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

题意：判断二叉树的最小深度

思路：
可以用递归去做，return min(left,right)


```
class Solution {
public:
//可以用递归去做，return min(left,right)
//数据:root为空的情况
//注意：当一个节点的左右子节点，有一个为空，一个不为空，那么为空的节点不能作为叶子计算路径，只有左右都为空才可以
    int minDepth(TreeNode *root) {
        if(root == NULL) return 0;
        //如果左右都为空，叶子节点
        if(root->left == NULL && root->right==NULL) return 1;
        //如果只有一个为空，返回另一个
        if(root->left == NULL) return (minDepth(root->right)+1);
        if(root->right == NULL) return (minDepth(root->left)+1);
        //如果两个都不为空，返回小的
        int left=minDepth(root->left)+1;
        int right=minDepth(root->right)+1;
        return left<=right?left:right;
    }
};
```
