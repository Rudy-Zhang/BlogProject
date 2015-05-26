title: "leetcode习题集 102 Binary Tree Level Order Traversal "
date: 2015-04-06 15:31:44
category: [算法]
tags: [leetcode习题集]
---
Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

For example:
Given binary tree {3,9,20,#,#,15,7},
```
	    3
	   / \
	  9  20
	    /  \
	   15   7
```
return its level order traversal as:
```
	[
	  [3],
	  [9,20],
	  [15,7]
	]
```
confused what "{1,#,2,3}" means? > read more on how binary tree is serialized on OJ.

题意：给定一个二叉树，输出层次遍历结果

思路：
二叉树层次遍历用队列

注意：要求每层一个vector，所以需要标记每一层的结束，通过加入一个NULL节点来判断



```
class Solution {
public:
//层次遍历，用队列
//需要记录申请新的vector的位置，使用一个NULL来标志需要新的vector了
    vector<vector<int> > levelOrder(TreeNode *root) {
        vector<vector<int> > vec;
        if(root==NULL) return vec;
        queue<TreeNode *> q;
        vector<int> vecl;
        q.push(root);
        q.push(NULL);
        while(!q.empty())
        {
            TreeNode *top=q.front();
            q.pop();
            if(top==NULL)
            {
                vec.push_back(vecl);
                vecl.resize(0);
                //只有q中还有元素的时候才压入，避免死循环
                if(q.size()>0)
                    q.push(NULL);
                
            }
            else
            {
                vecl.push_back(top->val);
                if(top->left!=NULL)
                    q.push(top->left);
                if(top->right!=NULL)
                    q.push(top->right);
            }
        }
        return vec;
        
    }
};

```
