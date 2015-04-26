title: "leetcode习题集 118 Pascal's Triangle II"
date: 2015-04-06 15:31:44
category: [leetcode习题集]
tags: []
---
Given an index k, return the kth row of the Pascal's triangle.

For example, given k = 3,
Return [1,3,3,1].

Note:
Could you optimize your algorithm to use only O(k) extra space?

题意：给定行数，生成帕斯卡三角，这一行的数字
提示：是否可以把额外存储优化到O(k)

思路：模拟,只能使用一行存储的话从左往右扫描是，res[i]+res[i+1]，会覆盖存储，但是从右往左扫描
res[i]+res[i-1]，不会覆盖存储

```
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        if(rowIndex<0)
        {
            vector<int> vec;
            return vec;
        }
        vector<int> vec(rowIndex+1,0);
        vec[0]=1;
        for(int i=1;i<=rowIndex;i++)
        {
            for(int j=i;j>=1;j--)
            {
                vec[j]=vec[j]+vec[j-1];
            }
        }
        return vec;
    }
};
```
