title: "leetcode习题集 39 Count and Say "
date: 2015-04-06 15:31:44
category: [leetcode习题集]
tags: []
---
The count-and-say sequence is the sequence of integers beginning as follows:
1, 11, 21, 1211, 111221, ...

1 is read off as "one 1" or 11.
11 is read off as "two 1s" or 21.
21 is read off as "one 2, then one 1" or 1211.
Given an integer n, generate the nth sequence.

Note: The sequence of integers will be represented as a string.

题意：数一列数，碰到n个一样的m输出nm，连续往下数

思路：模拟


```
class Solution {
public:
//使用笨方法模拟，碰到一个数字往后数相同的替换
    string countAndSay(int n) {
        string ans="1";
        if(n==1) return ans;
        for(int i=1;i<n;i++)
        {
            //做n-1次
            string temp=ans;
            //要把ans清空
            ans.resize(0);
            int j=0;
            for(;j<temp.size();)
            {
                int k=0;
                for(;j+k<temp.size() && temp[j+k]==temp[j];k++);
                /*{这部分逻辑有错误不是每次都操作
                    ans+=(k+'0');
                    ans+=(temp[j]+'0');本来就是asc码
                    
                }*/
                ans+=(k+'0');
                ans+=temp[j];
                j=j+k;//使用这个作为j的增加条件
            }
        }
        return ans;
    }
};
```