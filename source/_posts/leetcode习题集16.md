title: "leetcode习题集 16 3Sum Closest"
date: 2015-04-06 15:31:44
category: [leetcode习题集]
tags: []
---

Given an array S of n integers, find three integers in S such that the sum is closest to a given number, target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

    For example, given array S = {-1 2 1 -4}, and target = 1.

    The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).


题意：一组数字，其中的三个数字和最接近于target的这个和

思路：

K-Sum的问题

```
class Solution {
public:
//思路：退化成2sum问题
    int threeSumClosest(vector<int> &num, int target) {
        int twosum=0,left=0,right=0;
        int closest=INT_MAX;
        int ans=0;
        sort(num.begin(),num.end());
        for(int i=0;i+2<num.size();i++)
        {
            twosum=target-num[i];
            left=i+1;
            right=num.size()-1;
            while(left<right)
            {
                if(num[left]+num[right]+num[i]<target)
                {
                    if(target-num[left]-num[right]-num[i]<closest)
                    {
                        closest=target-num[left]-num[right]-num[i];
                        ans=num[left]+num[right]+num[i];              
                    }
                    left++;
                }
                else if(num[left]+num[right]+num[i]>target)
                {
                    if(num[left]+num[right]+num[i]-target<closest)
                    {
                        closest=num[left]+num[right]+num[i]-target;
                        ans=num[left]+num[right]+num[i];    
                    }
                    right--;
                }
                else
                {
                    return target;
                }
            }
        }
        return ans;
    }
};
```