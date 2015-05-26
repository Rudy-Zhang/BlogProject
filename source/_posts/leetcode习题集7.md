title: "leetcode习题集 7 Reverse Integer"
date: 2015-04-06 15:31:44
category: [算法]
tags: [leetcode习题集]
---

Reverse digits of an integer.

Example1: x = 123, return 321
Example2: x = -123, return -321

click to show spoilers.

Have you thought about this?
Here are some good questions to ask before coding. Bonus points for you if you have already thought through this!

If the integer's last digit is 0, what should the output be? ie, cases such as 10, 100.

Did you notice that the reversed integer might overflow? Assume the input is a 32-bit integer, then the reverse of 1000000003 overflows. How should you handle such cases?

For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

Update (2014-11-10):
Test cases had been added to test the overflow behavior.


题意：整数反转

思路：

1. %10，然后一位一位往上加

注意：

这题有个陷阱，反转整数可能出现整数溢出的情况，要考虑到


```
class Solution {
public:
//回文整数问题
    int reverse(int x) {
        long long int ans=0;
        while(x)
        {
            ans=ans*10+x%10;//这里不能写成ans+=ans*10+x%10
            x/=10;
        }
        if(ans>INT_MAX||ans<INT_MIN) return 0;
        else return ans;
    }
};
```