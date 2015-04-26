title: "leetcode习题集 110 Balanced Binary Tree"
date: 2015-04-06 15:31:44
category: [leetcode习题集]
tags: []
---
Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

题意：判断一个二叉树是不是平衡的

思路：
平衡二叉树：它是一棵空树或它的左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树。

根据平衡二叉树定义可以用递归解决


```
class Solution {
public:
	//二叉平衡树递归定义，所以可以用递归解决
	/*
	解法1，每次计算左右节点的深度，递归+递归，复杂度比较大
	解法2，使用map，记录每一个Node对应的深度
	*/
	//注意：root为空，左右节点有一个为空的情况
	map<TreeNode *,int> dic;
	bool isBalanced(TreeNode *root) {
		if(root==NULL) return true;
		int left=getLength(root->left);
		int right=getLength(root->right);
		if(abs(left-right)<=1 && isBalanced(root->left) && isBalanced(root->right))
			return true;
		else 
			return false;


	}

	int getLength(TreeNode *root)
	{
		if(root==NULL) return 0;
		if(dic.find(root)!=dic.end())
			return dic[root];
		int left=getLength(root->left)+1;
		int right=getLength(root->right)+1;
		dic[root]=(left>=right?left:right);
		return dic[root];
	}
};
```
