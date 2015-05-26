title: "leetcode习题集 26 Remove Duplicates from Sorted Array"
date: 2015-04-06 15:31:44
category: [算法]
tags: [leetcode习题集]
---

Given a sorted array, remove the duplicates in place such that each element appear only once and return the new length.

Do not allocate extra space for another array, you must do this in place with constant memory.

For example,
Given input array `A = [1,1,2]`,

Your function should return length = `2`, and A is now `[1,2]`.


题意：给定一个已经排好序的数组，除掉重复的元素

思路：
两个index，一个指向遍历的元素，一个指向下一个被覆盖的位置

注意：


```
class Solution {
public:
    int removeDuplicates(int A[], int n) {
        if(n==0) return 0;
        int cur=0,length=1;
        for(int i=1;i<n;i++)
        {
            if(A[i]==A[cur]) continue;
            else 
            {
                A[cur+1]=A[i];
                cur++;
                length++;
            }
            
        }
        return length;
    }
};
```