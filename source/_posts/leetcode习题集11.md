title: "leetcode习题集 11 Container With Most Water"
date: 2015-04-06 15:31:44
category: [leetcode习题集]
tags: []
---

Given n non-negative integers a1, a2, ..., an, where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container.


题意：n个非负整数，i代表横坐标，ai代表纵坐标，找到x轴和（0，ai）,（0，aj）组成的水池最大的容积

思路：

1. 暴力搜索O(n^2),这种解法往往会超时
2. 从两边往中间走，**取高度比较低的那一边**，比如只考虑左边，i1<i2
	* 如果a[i1]>a[i2]肯定不取a[i2]
	* 如果a[i1]<a[i2]
		* 如果S变小了,肯定不取a[i2]
		* 如果S变大了,肯定取a[i2]

为什么S变大了肯定取？
因为右边的墙壁向左移的所有的情况S都是一样的，都比新的S小，在这一刻被剪枝掉，**因为我们取的是高度比较低的那一个墙壁移动**。

这种方法的复杂度是O(n)


```
class Solution {
public:
    int maxArea(vector<int> &height) {
        int s=0,i=0,h=0,j=height.size()-1;
        while(i<j)
        {
            height[i]>height[j]?h=height[j]:h=height[i];
            int temps=(j-i)*h;
            if(temps>s) s=temps;
            if(height[i]<height[j])
                i++;
            else j--;
            
        }
        return s;
    }
};
```