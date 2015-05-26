title: "leetcode习题集 106 Construct Binary Tree from Inorder and Postorder Traversal"
date: 2015-04-06 15:31:44
category: [算法]
tags: [leetcode习题集]
---
Given inorder and postorder traversal of a tree, construct the binary tree.

Note:
You may assume that duplicates do not exist in the tree.

题意：根据后序遍历和中序遍历结果构建二叉树

思路：

1. 递归

后序遍历最后一个肯定是root
在inorder里面去找root
左边的是leftsubtree , 右边的是rightsubtree
然后计算左右子树的位置
递归构造tree就好了

2. 非递归思路：

（1）后续序列从后往前，添加到右子树并且入栈，直到碰到中序的下一个元素（从后往前），执行（2）

（2）pop栈顶元素，下一个元素添加到（pop之前的栈顶元素）左子树

（3）重复执行1,2直到后序遍历完毕

递归：

```
class Solution {
public:
    TreeNode *buildTree(vector<int> &inorder, vector<int> &postorder) {
		return create(inorder,0,inorder.size()-1,postorder,0,postorder.size()-1);
    }
private:
	TreeNode *create(vector<int> &inorder,int is,int ie, vector<int> &postorder,int ps,int pe)
	{
		if(ps>pe) return NULL;
		TreeNode *node=new TreeNode(postorder[pe]);
		int i;
		for(i=0;inorder[i]!=postorder[pe]&&i<inorder.size();i++);
		node->left=create(inorder,is,i-1,postorder,ps,ps+i-is-1);
		node->right=create(inorder,i+1,ie,postorder,ps+i-is,pe-1);
		return node;
	}
};
```
非递归
```
class Solution2 {
public:
    TreeNode *buildTree(vector<int> &inorder, vector<int> &postorder) {
		if(inorder.size()==0||postorder.size()==0) return NULL;
		int i=postorder.size()-1;
		int j=inorder.size()-1;
		int flag=0;
		stack<TreeNode *> stack;

		TreeNode *head=new TreeNode(postorder[i]);
		TreeNode *node=head;
		stack.push(node);
		i--;
		while(i>=0)
		{
			if(!stack.empty()&&stack.top()->val==inorder[j])
			{
				node=stack.top();
				j--;
				stack.pop();
				flag=1;
			}
			else
			{
				if(flag==0)
				{
					node->right=new TreeNode(postorder[i]);
					node=node->right;
					stack.push(node);
					i--;
				}
				else
				{
					node->left=new TreeNode(postorder[i]);
					node=node->left;
					stack.push(node);
					i--;
					flag=0;
				}
			}
		}
		return head;
    }

};
```