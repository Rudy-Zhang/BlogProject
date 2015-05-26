title: "leetcode习题集 107 Binary Tree Level Order Traversal II "
date: 2015-04-06 15:31:44
category: [算法]
tags: [leetcode习题集]
---
Given a binary tree, return the bottom-up level order traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).

For example:
Given binary tree {3,9,20,#,#,15,7},
```
    3
   / \
  9  20
    /  \
   15   7
```
return its bottom-up level order traversal as:
```
[
  [15,7],
  [9,20],
  [3]
]
```
confused what `"{1,#,2,3}"` means? > read more on how binary tree is serialized on OJ.

题意：层次遍历二叉树，从底向上输出

思路：用队列，再用栈反转一下




```
class Solution {
public:
//层次遍历，用队列
//需要记录申请新的vector的位置，使用一个NULL来标志需要新的vector了
//使用stack，翻转
    vector<vector<int> > levelOrderBottom(TreeNode *root) {
        vector<vector<int> > vec;
        if(root==NULL) return vec;
        stack<vector<int> > stack;
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
                stack.push(vecl);
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
        while(!stack.empty())
        {
            vecl=stack.top();
            stack.pop();
            vec.push_back(vecl);
        }
        return vec;
        
    }
};
```
