title: "leetcode习题集 125 Valid Palindrome"
date: 2015-04-06 15:31:44
category: [leetcode习题集]
tags: []
---
Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

For example,
`"A man, a plan, a canal: Panama"` is a palindrome.
`"race a car"` is not a palindrome.

Note:
Have you consider that the string might be empty? This is a good question to ask during an interview.

For the purpose of this problem, we define empty string as valid palindrome.


题意：给定一个字符串判断是不是回文字符串


思路：模拟

注意：

字符串为空
中间的非字母,数字的char不比较，包括开头和结束
在for内部对ij操作要防止越界

```
class Solution {
public:
    bool isPalindrome(string s) {
        if(s.empty()) return true;
        int i=0,j=s.size()-1;
        for(;i<s.size();i++,j--)
        {
            while(i<s.size() && !ischar(s[i])) i++;
            while(j>=0 && !ischar(s[j])) j--;
            if(i==s.size() || j==-1) 
                break;
            if(tolower(s[i])!=tolower(s[j])) 
                return false;
        }
        while(i<s.size() && !ischar(s[i])) i++;
        while(j>=0 && !ischar(s[j])) j--;
        if(i==s.size()&&j==-1) return true;
        else return false;
    }
    
    bool ischar(char a)
    {
        return ((a>='A'&&a<='Z')||(a>='a'&&a<='z')||(a>='0'&&a<='9'));
    }
    
};
```
