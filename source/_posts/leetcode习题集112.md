title: "leetcode习题集 112 Path Sum"
date: 2015-04-06 15:31:44
category: [leetcode习题集]
tags: []
---
Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.

For example:
Given the below binary tree and `sum = 22`,
```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1
```
return true, as there exist a root-to-leaf path `5->4->11->2` which sum is 22.

题意：给定一个二叉树，判断是否有一个根到叶的路径和为sum

思路：
可以使用递归


```
class Solution {
public:
//可以使用递归
//数据：root为空的情况
//root==NULL,sum==0为假
    bool hasPathSum(TreeNode *root, int sum) {
        if(root==NULL)
            return false;
        //当左右节点都为空且节点值等于sum
        if(root->val == sum && root->left==NULL && root->right==NULL) 
            return true;
        if(hasPathSum(root->left,sum-root->val))
            return true;
        else if(hasPathSum(root->right,sum-root->val))
            return true;
        else 
            return false;
    }
};
```
