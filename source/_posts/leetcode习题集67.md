title: "leetcode习题集 67 Add Binary "
date: 2015-04-06 15:31:44
category: [leetcode习题集]
tags: []
---
Given two binary strings, return their sum (also a binary string).

For example,
a = `"11"`
b = `"1"`
Return `"100"`.

题意：给定一个int的vector，给这个数字加上1，返回最终的数字

思路：模拟

注意：

```
class Solution {
public:
//思路1：stoi转化为整数，转换进位，itos转化为字符串，比较复杂
//思路2：直接使用字符从后向前加，carry记录
//应该先补齐短字符串的长度
//数据：字符串为空，字符串首位有进位
    string addBinary(string a, string b) {
        if(a.size()==0) return b;
        if(b.size()==0) return a;
        string ans;
        int carry=0;
        int i=a.size()-1,j=b.size()-1;
        for(;i>=0&&j>=0;i--,j--)
        {
            ans=add(a[i],b[j],carry)+ans;
        }
        string sub;
        if(i>=0)
            sub=a.substr(0,i+1);
        else if(j>=0)
            sub=b.substr(0,j+1);
        else
            sub="";
        if(carry==0)
            return (sub+ans);
        else if(carry==1 && i<0 && j<0)
            return ('1'+ans);
        else 
        {
            for(int k=sub.size()-1;k>=0;k--)
            {
                ans=add('0',sub[k],carry)+ans;
            }    
            if(carry==1)
                return ('1'+ans);
            else
                return ans;
            
        }
        
    }
    
    char add(char a,char b,int &carry)
        {
            if(a=='0'&&b=='0') 
			{
				if(carry==1) 
				{
					carry=0;
					return '1';
				}
				else return '0';
			}
            if((a=='0'&&b=='1')||(a=='1'&&b=='0')) 
            {
                if(carry==0) return '1';
                if(carry==1)
                {
                    carry==1;
                    return '0';
                }
            }
            if(a=='1'&&b=='1')
            {
                if(carry==0)
                {
                    carry=1;
                    return '0';
                }
                if(carry==1)
                {
                    carry=1;
                    return '1';
                }
            }
            
        }
};
```