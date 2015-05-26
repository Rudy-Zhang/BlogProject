title: "leetcode习题集 15 3Sum   "
date: 2015-04-06 15:31:44
category: [算法]
tags: [leetcode习题集]
---

Given an array S of n integers, are there elements a, b, c in S such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

Note:
Elements in a triplet (a,b,c) must be in non-descending order. (ie, a ≤ b ≤ c)
The solution set must not contain duplicate triplets.
For example, given array S = {-1 0 1 2 -1 -4},

    A solution set is:
    (-1, 0, 1)
    (-1, -1, 2)


题意：找出一组数字，其中的三个数字和等于0，所有的情况

思路：

1. 暴力解法，遍历，复杂度O(n^3)，超时，必须使用O(n^2)的解法
2. 这是一个K-Sum的问题，目标可以等于0也可以等于任何数字，所有的K-Sum问题暴力解法都有O(n^k)的复杂度，我们可以在遍历i的时候使用target-num[i]然后使用2-Sum在O(n)复杂度的解法就可以了。所以，K-Sum的问题，都可以优化到O(n^(k-1))的复杂度解决 

注意：如何避免重复，是K-Sum中很关键的问题，可以使用set避免重复，但是本题中使用set也会超时，所以只能手动避免重复情况发生。

[更多K-Sum问题的资料](http://tech-wonderland.net/blog/summary-of-ksum-problems.html)


```
class Solution {
public:
//先排序，然后遍历O(n^3),超时，必须使用O(n^2)的解法
//k-sum问题需要优化到O(n^(k-1))，先排序，再退化成2sum问题
//注意不能重复，使用set,导致操作过多，超时
//手动避免重复
//参考
    vector<vector<int> > threeSum(vector<int> &num) {
        vector<vector<int>> vecs;
        sort(num.begin(),num.end());
        for(int i=0;i+2<num.size();i++)
        {
            int target=-num[i];
            int left=i+1,right=num.size()-1;
            while(left<right)
            {
                int sum=num[left]+num[right];
                if(sum==target)
                {
                    vector<int> vec;
                    int templeft=num[left],tempright=num[right];
                    vec.push_back(num[i]);
                    vec.push_back(num[left]);
                    vec.push_back(num[right]);
                    vecs.push_back(vec);
					while(left<right && num[left]==templeft) left++;
					while(left<right && num[right]==tempright) right--;
                }
                else if(sum<target)
                    left++;
                else if(sum>target)
                    right--;
                //这一句很关键，不然会超时    
                while (i + 1 < num.size() && num[i + 1] == num[i]) 
                    i++;
                
            }
        }
        return vecs;
    }
};
```