title: "leetcode习题集 14 Longest Common Prefix  "
date: 2015-04-06 15:31:44
category: [leetcode习题集]
tags: []
---

Write a function to find the longest common prefix string amongst an array of strings.


题意：找出一组字符串的最长相同前缀

思路：

找出strs最小长度，遍历即可




```
class Solution {
public:
//找出最小长度
    string longestCommonPrefix(vector<string> &strs) {
        string prefix;
        if(strs.size()==0) return prefix;
        int minLength=strs[0].size();
        for(int i=0;i<strs.size();i++)
        {
            if(strs[i].size()<minLength)
                minLength=strs[i].size();
        }
        
        for(int j=0;j<minLength;j++)
        {
            for(int i=0;i<strs.size()-1;i++)
            {
                if(strs[i][j]!=strs[i+1][j]) return prefix;
            }
            prefix+=strs[0][j];
        }
        return prefix;
    }
};
```