title: "leetcode习题集 6 ZigZag Conversion"
date: 2015-04-06 15:31:44
category: [leetcode习题集]
tags: []
---

The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

	P   A   H   N
	A P L S I I G
	Y   I   R

And then read line by line: `"PAHNAPLSIIGYIR"`
Write the code that will take a string and make this conversion given a number of rows:

	string convert(string text, int nRows);
`convert("PAYPALISHIRING", 3)` should return `"PAHNAPLSIIGYIR"`.

题意：zigzag斜线方向的问题，给定竖向扫描字符串和行数，返回横向的字符串

思路：

1. 暴力解法，声明一个nRow的字符串数组，每个字符串统计这行的内容。模拟扫描

注意：

考虑`nRow=1`的情况


```
class Solution {
public:
    string convert(string s, int nRows) {
        if(nRows==1) return s;//考虑nRows等于1的情况
        string *strs=new string[nRows];
        int rowIndex=0,direction=1;
        for(int i=0;i<s.size();i++)
        {
            strs[rowIndex]+=s[i];
            rowIndex+=direction;
            if(rowIndex==nRows-1) direction=-1;
            if(rowIndex==0) direction=1;
        }
        s.clear();
        for(int i=0;i<nRows;i++)
            s+=strs[i];
        delete[] strs;//这里要用delete[] str删除数组
        return s;
    }
};
```