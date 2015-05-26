title: "leetcode习题集 100 Same Tree"
date: 2015-04-06 15:31:44
category: [算法]
tags: [leetcode习题集]
---
Given two binary trees, write a function to check if they are equal or not.

Two binary trees are considered equal if they are structurally identical and the nodes have the same value.

题意：给定两个二叉树，判断两个二叉树是不是一样的（树结构相同，节点值相同）

思路：

1. 先序中序递归遍历，如果两个序列都相等，则一定相同
2. 先序递归遍历
3. 先序非递归
4. 层次遍历

先序递归遍历

```
bool isSameTree(TreeNode *p, TreeNode *q) {
	if(NULL==p && NULL==q) return true;
	if(NULL==p && NULL!=q) return false;
	if(NULL!=p && NULL==q) return false;
	if(p->val != q->val) return false;
	return (isSameTree(p->left,q->left)&&isSameTree(p->right,q->right));
}
```
层次遍历
```
/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    /*bool isSameTree(TreeNode *p, TreeNode *q) {
        if(NULL==p && NULL==q) return true;
        if(NULL==p && NULL!=q) return false;
        if(NULL!=p && NULL==q) return false;
        if(p->val != q->val) return false;
        return (isSameTree(p->left,q->left)&&isSameTree(p->right,q->right));
    }*/
    bool isSameTree(TreeNode *p, TreeNode *q) {
        queue<TreeNode *> queue1,queue2;
        if(NULL == p && NULL==q) return true;
        if(NULL != p && NULL==q) return false;
        if(NULL == p && NULL!=q) return false;
        queue<TreeNode *> q1,q2;
        q1.push(p);
        q2.push(q);
        TreeNode *n1,*n2;
        while(!q1.empty() && !q2.empty())
        {
            n1=q1.front();
            q1.pop();
            n2=q2.front();
            q2.pop();
            if(NULL==n1 && NULL==n2)
                continue;
            if(NULL==n1 && NULL!=n2)
                return false;
            if(NULL!=n1 && NULL==n2)
                return false;
            if(n1->val != n2->val)
                return false;
            q1.push(n1->left);
            q1.push(n1->right);
            q2.push(n2->left);
            q2.push(n2->right);
        }
        if(!(q1.empty()&&q2.empty()))
            return false;
        return true;
    }
};
```