title: "leetcode习题集 9 Palindrome Number"
date: 2015-04-06 15:31:44
category: [leetcode习题集]
tags: []
---

Determine whether an integer is a palindrome. Do this without extra space.

click to show spoilers.

Some hints:
Could negative integers be palindromes? (ie, -1)

If you are thinking of converting the integer to string, note the restriction of using extra space.

You could also try reversing an integer. However, if you have solved the problem "Reverse Integer", you know that the reversed integer might overflow. How would you handle such case?

There is a more generic way of solving this problem.


题意：判断一个整数是不是回文数

思路：

1. 和反转整数的思路一样，反转到一半进行比较（其实反转到比较能相等就可以了）。需要考虑比较多的是测试用例的情况

注意：

1. 负数都不是回文数
2. 10的倍数不能当做回文数，计算时翻转0就消失了
3. 当然0是回文数
4. 整数变化问题都要考虑是否越界,翻转其实只需要做到一半比较就可以了
5. 考虑中间有一位，和中间有两位的情况



```
public:
    bool isPalindrome(int x) {
        if(x==0) return true;
        if(x<0 || (x%10==0)) return false;
        int y=0;
        while(y<x)
        {
            y=y*10+x%10;
            if(x==y) return true;
            x/=10;
            if(x==y) return true;
        }
        return (x==y);
    }
};
```