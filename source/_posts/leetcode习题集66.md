title: "leetcode习题集 66 Plus One"
date: 2015-04-06 15:31:44
category: [算法]
tags: [leetcode习题集]
---
Given a non-negative number represented as an array of digits, plus one to the number.

The digits are stored such that the most significant digit is at the head of the list.

题意：给定一个int的vector，给这个数字加上1，返回最终的数字

思路：模拟

注意：
```
class Solution {
public:
    vector<int> plusOne(vector<int> &digits) {
        int length=digits.size();
        int i;
        for(i=length-1;i>=0;i--)
        {
            digits[i]++;
            if(digits[i]<10) break;
            else digits[i]=0;
        }
        
        if(i<0)
            digits.insert(digits.begin(),1);
        return digits;
    }
};
```