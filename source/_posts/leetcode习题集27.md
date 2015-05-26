title: "leetcode习题集 27 Remove Element"
date: 2015-04-06 15:31:44
category: [算法]
tags: [leetcode习题集]
---

Given an array and a value, remove all instances of that value in place and return the new length.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.


题意：给定一个数组和一个值，移除数组中和这个值相等的元素，新长度后面的元素无所谓，返回数组新长度

思路：
遍历，碰到一样的把最后一个元素覆盖上来

注意：


```
class Solution {
public:
    int removeElement(int A[], int n, int elem) {
        for (int i = 0; i < n; i++)
		{
			if(A[i]==elem)
			{
				A[i]=A[n-1];
				n--;
				i--;
			}
		}
		return n;
    }
};
```