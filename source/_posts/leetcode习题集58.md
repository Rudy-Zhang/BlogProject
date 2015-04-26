title: "leetcode习题集 58 Length of Last Word"
date: 2015-04-06 15:31:44
category: [leetcode习题集]
tags: []
---
Given a string s consists of upper/lower-case alphabets and empty space characters ' ', return the length of last word in the string.

If the last word does not exist, return 0.

Note: A word is defined as a character sequence consists of non-space characters only.

For example, 
Given s = "Hello World",
return 5.

题意：给定一个字符串，返回字符串中最后一个单词的长度

思路：模拟

注意：最后面有空格
```
class Solution {
public:
//不使用strlen一次遍历得出结果
//数据：s==NULL,开头结束有空格!所以不能以碰到' '把len归零，需要以普通字符开始为准
    int lengthOfLastWord(const char *s) {
        int len=0;
        while(*s!='\0')
        {
            if(*s!=' ')
            {
                len=0;
                while(*s && *s!=' ')
                {
                    len++;
                    s++;
                }
            }
            else
                s++;
        }
        return len;
    }
};
```