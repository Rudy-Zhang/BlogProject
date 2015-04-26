title: "leetcode习题集 134 Gas Station"
date: 2015-04-06 15:31:44
category: [leetcode习题集]
tags: []
---
There are N gas stations along a circular route, where the amount of gas at station i is gas[i].

You have a car with an unlimited gas tank and it costs cost[i] of gas to travel from station i to its next station (i+1). You begin the journey with an empty tank at one of the gas stations.

Return the starting gas station's index if you can travel around the circuit once, otherwise return -1.

Note:
The solution is guaranteed to be unique.


题意：给N个加油站的油量gas[i],N段路的费油量cost[i],形成一个圈，判断能否从一个点开始绕这个圈走一圈。如果可以，输出点的位置，不可以输出-1


思路：

遍历开始的节点，模拟，复杂度O(n^2),超时
思考
gas-cost形成的循环数组，如果总和大于0一定可以走通
如果A到B，花费大于油量，肯定走不通，A到B走不通，那么AB之间的节点到B也走不通都不能作为起点



```
class Solution {
public:
    int canCompleteCircuit(vector<int> &gas, vector<int> &cost) {
        int j=0,tank=0,total=0;
        for(int i=0;i<gas.size();i++)
        {
            tank+=(gas[i]-cost[i]);
            total+=gas[i]-cost[i];
            if(tank<0)
            {
                j=i+1;
                tank=0;
            }
        }
        return (total>=0)?j:-1;
    }
};
```
