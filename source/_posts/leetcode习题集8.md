title: "leetcode习题集 8 String to Integer (atoi) "
date: 2015-04-06 15:31:44
category: [算法]
tags: [leetcode习题集]
---

Implement atoi to convert a string to an integer.

Hint: Carefully consider all possible input cases. If you want a challenge, please do not see below and ask yourself what are the possible input cases.

Notes: It is intended for this problem to be specified vaguely (ie, no given input specs). You are responsible to gather all the input requirements up front.

spoilers alert... click to show requirements for atoi.

Requirements for atoi:
The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.

The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.

If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.

If no valid conversion could be performed, a zero value is returned. If the correct value is out of the range of representable values, INT_MAX (2147483647) or INT_MIN (-2147483648) is returned.


题意：实现C++库函数（在cstdlib中）atoi，转化string为整数

思路：

1. 开头可能是正负号，然后以数字的字符开始，到第一个不是数字字符结束，忽略后面的内容。如果字符串不合法，返回0；

注意：

1. 处理开头为空的情况

2. 开头有可能是负号,正号

3. 注意测试用例完备，超过int_max小于int_min



```
class Solution {
public:
    int atoi(string str) {
        int emptyIndex=0;
        while(str[emptyIndex]==' ') emptyIndex++;
        str=str.substr(emptyIndex);
        
        bool isneg=false;
        if(!isNum(str[0]) && str[0]!='-' && str[0]!='+') return 0;
        if(str[0]=='-')
        {
          isneg=true;
          str=str.substr(1);
        } 
        else if(str[0]=='+')
            str=str.substr(1);
        long long num=0;
        for(int i=0;isNum(str[i]);i++)
        {
            num=num*10+(str[i]-'0');
            if(isneg==false&&num>INT_MAX) return INT_MAX;
            if(isneg==true&&(0-num)<INT_MIN) return INT_MIN;
        }
        if(isneg==true) return (int)(0-num);
        else return (int)num;
        
    }
    
    bool isNum(char a)
    {
        if((a-'0')>=0 && (a-'0')<10)
            return 1;
        else return 0;
    }
};
```