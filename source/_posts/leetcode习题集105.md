title: "leetcode习题集 105 Construct Binary Tree from Preorder and Inorder Traversal"
date: 2015-04-06 15:31:44
category: [算法]
tags: [leetcode习题集]
---
Given preorder and inorder traversal of a tree, construct the binary tree.

Note:
You may assume that duplicates do not exist in the tree.

题意：根据先序遍历和中序遍历结果构建二叉树

思路：

1. 递归

前序遍历第一个肯定是root
在inorder里面去找root
左边的是leftsubtree , 右边的是rightsubtree
然后preorder除去order,后面lifetsubtree.size 个是左边的, rightsubtree.size个是右边的
递归构造tree就好了

1. 非递归
（1）先序的序列不断添加到左子树，并入栈，直到碰到中序的下一个元素执行（2）
（2）pop栈顶元素，下一个元素添加到右子树
（3）重复执行1,2直到先序序列遍历完毕

递归：

```
class Solution {
public:
    TreeNode *buildTree(vector<int> &preorder, vector<int> &inorder) {
		return make(preorder,0,preorder.size()-1,inorder,0,inorder.size()-1);
    }
private :
	TreeNode *make(vector<int> &preorder,int ps,int pe,vector<int> &inorder,int is,int ie)
	{
		if(ps>pe) return NULL;
		TreeNode *node=new TreeNode(preorder[ps]);
		int i;
		for(i=is;i<inorder.size()&&inorder[i]!=preorder[ps];i++);
		node->left=make(preorder,ps+1,ps+i-is,inorder,is,i-1);
		node->right=make(preorder,ps+i-is+1,pe,inorder,i+1,ie);
		return node;
	}
};
```
非递归
```
class Solution{
public:
    TreeNode *buildTree(vector<int> &preorder, vector<int> &inorder) {
		if(preorder.size()==0) return NULL;
		stack<TreeNode *> stack;
		TreeNode *node=new TreeNode(preorder[0]);
		TreeNode *head=node;
		stack.push(node);
		int i=1,j=0,flag=0;
		while(i<preorder.size())
		{
			if(!stack.empty()&&stack.top()->val==inorder[j])
			{
				node=stack.top();
				stack.pop();
				flag=1;
				j++;
			}
			else
			{
				if(flag==0)
				{
					node->left=new TreeNode(preorder[i]);
					node=node->left;
					stack.push(node);
					i++;
				}
				else
				{
					node->right=new TreeNode(preorder[i]);
					node=node->right;
					stack.push(node);
					i++;
					flag=0;
				}
			}
		}
		return head;

    }

};
```