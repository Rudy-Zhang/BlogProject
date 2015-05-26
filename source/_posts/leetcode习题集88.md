title: "leetcode习题集 88 Merge Sorted Array "
date: 2015-04-06 15:31:44
category: [算法]
tags: [leetcode习题集]
---
Given two sorted integer arrays A and B, merge B into A as one sorted array.

Note:
You may assume that A has enough space (size that is greater or equal to m + n) to hold additional elements from B. The number of elements initialized in A and B are m and n respectively.

题意：给两个有序数组，合并成一个

思路：模拟


```
class Solution {
public:
    void merge(int A[], int m, int B[], int n) {
        int i=m-1,j=n-1,k=m+n-1;
		while(i>=0&&j>=0)
		{
			if(A[i]>=B[j])
			{ 
				A[k--]=A[i];
				i--;	
			}
			else
			{
				A[k--]=B[j];
				j--;
			} 
		}
		if(j>=0)
			for(;k>=0;k--)
				A[k]=B[k];
    }
};
```
