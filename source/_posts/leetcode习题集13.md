title: "leetcode习题集 13 Roman to Integer "
date: 2015-04-06 15:31:44
category: [算法]
tags: [leetcode习题集]
---

Given a roman numeral, convert it to an integer.

Input is guaranteed to be within the range from 1 to 3999.

题意：把罗马数字转化成整数

思路：

解这个题必须先理解罗马数字计数方式，罗马数字是从大往小排的，但是有可能出现反序的情况如IV表示`5-1=4`，不是反序的情况只要把所有数字加起来就可以了。




```
class Solution {
public:
    int romanToInt(string s) {
		map<char,int> dct;
		dct['I'] = 1, dct['i'] = 1;
		dct['V'] = 5, dct['v'] = 5;
		dct['X'] = 10, dct['x'] = 10;
		dct['L'] = 50, dct['l'] = 50;
		dct['C'] = 100, dct['c'] = 100;
		dct['D'] = 500, dct['d'] = 500;
		dct['M'] = 1000, dct['m'] = 1000;
		int sum=0;
		for(int i=0;i<s.size();i++)
		{
			int j=i+1;
			if(j<s.size() && dct[s[i]]<dct[s[j]])
			{
				sum+=dct[s[j]]-dct[s[i]];
				i++;
			}
			else
			{
				sum+=dct[s[i]];
			}
		}
		return sum;
    }
};
```