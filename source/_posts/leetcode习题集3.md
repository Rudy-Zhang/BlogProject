title: "leetcode习题集 3 Longest Substring Without Repeating Characters"
date: 2015-04-06 15:31:44
category: [leetcode习题集]
tags: []
---

Given a string, find the length of the longest substring without repeating characters. For example, the longest substring without repeating letters for "abcabcbb" is "abc", which the length is 3. For "bbbbb" the longest substring is "b", with the length of 1.

题意：给定一个字符串，找到最长不重复字母的字串

思路：

1. 暴力搜索，遍历字符串，以每个字符串开头向后找非重复的串碰到重复停止，使用map记录，降低复杂度到n^2,还是超时了

2. 使用int[256]数组记录每个字符上次出现的位置，start记录字串的起始位置，碰到有记录的start变为记录位置+1，只扫描一遍，复杂度O(n)



```
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        vector<int> vec(256,-1);
        int start=0;
        int max=0;
        for(int i=0;i<s.size();i++)
        {
            if(vec[s[i]]>=start)
            {
                start=vec[s[i]]+1;
            }
            vec[s[i]]=i;
            if(i-start+1>max)
                max=i-start+1;
        }
        return max;
    }
};
```