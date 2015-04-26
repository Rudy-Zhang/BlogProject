title: "leetcode习题集 118 Pascal's Triangle "
date: 2015-04-06 15:31:44
category: [leetcode习题集]
tags: []
---
Given numRows, generate the first numRows of Pascal's triangle.

For example, given numRows = 5,
Return
```
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```

题意：给定行数，生成帕斯卡三角

思路：模拟


```
class Solution {
public:
    vector<vector<int> > generate(int numRows) {
        vector<vector<int>> arrs;
		for (int i = 0; i < numRows; i++)
		{
			vector<int> row;
			if(i==0)
			{
				vector<int> firstRow;
				firstRow.push_back(1);
				arrs.push_back(firstRow);
				continue;
			}
			for(int j=0;j<=i;j++)
			{
				int left,right;
				if(j-1<0) left=0;
				else left=arrs[i-1][j-1];
				if(j+1>arrs[i-1].size()) right=0;
				else right=arrs[i-1][j];
				row.push_back(left+right);
			}
			arrs.push_back(row);
		}
		return arrs;
    }
};
```
