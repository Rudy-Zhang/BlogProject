title: "leetcode习题集 70 Climbing Stairs"
date: 2015-04-06 15:31:44
category: [leetcode习题集]
tags: []
---
You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

题意：爬楼梯，每次可以爬一步或者两步，爬到第n阶台阶有几种方法

思路：
类似于斐波那契数列，f(n)=f(n-1)+f(n-2),使用a[n]空间换时间，否则递归太多容易爆栈
实际上这是一个动态规划问题

注意：

```
class Solution {
public:
//类似于斐波那契数列，f(n)=f(n-1)+f(n-2),使用a[n]空间换时间，否则递归太多容易爆栈
    int climbStairs(int n) {
        int *a=new int[n+1];
        for(int i=0;i<n+1;i++)
            a[i]=0;
        a[1]=1;
        a[2]=2;
        count(a,n);
    }
    
    int count(int a[],int n)
    {
        if(a[n]!=0) return a[n];
        else 
        {
            int num=count(a,n-1)+count(a,n-2);
            a[n]=num;
            return num;
        }
    }
};

```