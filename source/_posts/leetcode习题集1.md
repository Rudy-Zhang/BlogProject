title: "leetcode习题集 1 Two Sum"
date: 2015-04-06 15:31:44
category: [算法]
tags: [leetcode习题集]
---

Given an array of integers, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2. Please note that your returned answers (both index1 and index2) are not zero-based.

You may assume that each input would have exactly one solution.

Input: numbers={2, 7, 11, 15}, target=9
Output: index1=1, index2=2

题意：找vector中的两个数，和为target

思路：

1. 两重循环遍历，复杂度O(n^2)

2. 先排序，标记left，right，扫描一遍即可，复杂度O(nlogn)

3. 空间换时间,每次访问一个数记下这个数的index和值，使用hash表存储，下次需要的时候直接取出来复杂度O(n)

```
class Solution {
public:
    vector<int> twoSum(vector<int> &numbers, int target) {
		vector<int> vec;
		unordered_map<int,int> hash;
		for(int i=0;i<numbers.size();i++)
		{
			int numToFind=target-numbers[i];
			if(hash.find(numToFind)!=hash.end())
			{
				vec.push_back(hash[numToFind]+1);
				vec.push_back(i+1);
				return vec;
			}
			else
				hash[numbers[i]]=i;
		}
    }
};
```