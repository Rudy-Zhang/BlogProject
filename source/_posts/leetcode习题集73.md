title: "leetcode习题集 73 Set Matrix Zeroes"
date: 2015-04-06 15:31:44
category: [算法]
tags: [leetcode习题集]
---
Given a m x n matrix, if an element is 0, set its entire row and column to 0. Do it in place.

click to show follow up.

Follow up:
Did you use extra space?
A straight forward solution using O(mn) space is probably a bad idea.
A simple improvement uses O(m + n) space, but still not the best solution.
Could you devise a constant space solution?

题意：给定一个m*n的矩阵，如果有一个元素等于0，那么这个元素属于的行和列都变成0

思路：

1. 使用另外的存储m*n矩阵记录遍历结果，不太好
2. 使用M+N的存储记录遍历结果，指示第M[i]行和第N[j]列是否被置0
3. 使用row0，col0记录第一行，第一列的状态，然后把matrix[i][j]映射到第一行第一列，然后遍历，根据第一行，第一列的情况为矩阵置0，处理第一行第一列。使用了常数存储，两次遍历。

M+N存储

```
class Solution {
public:
    void setZeroes(vector<vector<int> > &matrix) {
        vector<bool> vecM(matrix.size(),false);
		vector<bool> vecN(matrix[0].size(),false);
		for(int i=0;i<matrix.size();i++)
		{
			for(int j=0;j<matrix[i].size();j++)
			{
				if(matrix[i][j]==0){
					vecM[i]=true;
					vecN[j]=true;
				}
			}
		} 
		for(int i=0;i<vecM.size();i++)
			if(vecM[i]==true)
				for(int j=0;j<matrix[i].size();j++)
					matrix[i][j]=0;
		for(int i=0;i<vecN.size();i++)
			if(vecN[i]==true)
				for(int j=0;j<matrix.size();j++)
					matrix[j][i]=0;
    }
};

```
常数存储
```
class Solution {
public:
    void setZeroes(vector<vector<int> > &matrix) {
        int col0=0,row0=0,rows = matrix.size(), cols = matrix[0].size();

		for(int j=0;j<cols;j++)
			if(matrix[0][j]==0) row0=1;

		for(int i=0;i<rows;i++)
		{
			if(matrix[i][0]==0) col0=1;
			for(int j=0;j<cols;j++)
			{
				if(matrix[i][j]==0) 
					matrix[i][0]=matrix[0][j]=0;
			}
		}
		for(int i=1;i<rows;i++)
		{
			for(int j=1;j<cols;j++)
			{
				if(matrix[i][0]==0||matrix[0][j]==0) 
					matrix[i][j]=0;
			}
		}
		if(col0==1)
			for(int i=0;i<rows;i++)
				matrix[i][0]=0;
		if(row0==1)
			for(int j=0;j<cols;j++)
				matrix[0][j]=0;
    }
};
```