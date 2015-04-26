title: "leetcode习题集 101 Symmetric Tree"
date: 2015-04-06 15:31:44
category: [leetcode习题集]
tags: []
---
Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree is symmetric:
```

	    1
	   / \
	  2   2
	 / \ / \
	3  4 4  3
```
But the following is not:
```
	    1
	   / \
	  2   2
	   \   \
	   3    3   					 
```
Note:
Bonus points if you could solve it both recursively and iteratively.

confused what `"{1,#,2,3}"` means? > read more on how binary tree is serialized on OJ.

题意：给定一个二叉树，判断二叉树是不是对称的

思路：

1. 递归遍历，如果两个节点对称，那么，1）值相等，2）左左右右对称，左右右左对称
2. 使用队列进行层次遍历，每次弹出队列的时候比较，注意子节点为空不为空的比较

注意：思路一根节点拆分成两个节点

先序递归遍历
```
class Solution {
public:
    bool isSymmetric(TreeNode *root) {
        if(root==NULL) return true;
        else return isSym(root->left,root->right);
    }
    
    bool isSym(TreeNode *left,TreeNode *right)
    {
        if(left==NULL && right==NULL) return true;
        if(left==NULL && right!=NULL) return false;
        if(left!=NULL && right==NULL) return false;
        if(left->val==right->val && isSym(left->left,right->right) && isSym(left->right,right->left))
            return true;
    }
};
}
```
层次遍历
```
class Solution {
public:
    bool isSymmetric(TreeNode *root) {
        queue<TreeNode *> q1,q2;
        if(root==NULL) return true;
        if(NULL != root->left && NULL==root->right ) return false;
        if(NULL ==root->left && NULL!=root->right) return false;
        q1.push(root);
        q2.push(root);
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
            q2.push(n2->right);
            q2.push(n2->left);
        }
        if(!(q1.empty()&&q2.empty()))
            return false;
        return true;
        
    }
    
    
};
```