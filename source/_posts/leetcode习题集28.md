title: "leetcode习题集 27 Remove Element"
date: 2015-04-06 15:31:44
category: [算法]
tags: [leetcode习题集]
---

Implement strStr().

Returns the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.


题意：实现strstr()函数，返回第二个字符串在第一个字符串中出现的index

思路：模拟

注意：

1. 两个指针指向NULL的情况
2. 后面字符串比前面长的情况
3. 使用strlen会超时,必须只能扫描一次
4. '\0'的asc码值是0，*needle==0判断结束

```
class Solution {
public:
    int strStr(char *haystack, char *needle) {
        if(NULL==haystack || NULL==needle) return -1;
        if(*needle==0) return 0;
        for(int i=0;haystack[i]!='\0';i++)
        {
            if(haystack[i]==needle[0])
            {
                int j;
                for(j=0;needle[j]!='\0';j++)
                {
                    if(haystack[i+j]=='\0')
                        return -1;
                    if(haystack[i+j]!=needle[j])
                        break;
                }
                if(needle[j]=='\0')
                    return i;
            }
        }
        return -1;
    }
};
```