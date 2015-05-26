title: "leetcode习题集 125 Clone Graph"
date: 2015-04-06 15:31:44
category: [算法]
tags: [leetcode习题集]
---
Clone an undirected graph. Each node in the graph contains a label and a list of its neighbors.


OJ's undirected graph serialization:
Nodes are labeled uniquely.

We use # as a separator for each node, and , as a separator for node label and each neighbor of the node.
As an example, consider the serialized graph {0,1,2#1,2#2,2}.

The graph has a total of three nodes, and therefore contains three parts as separated by #.

First node is labeled as 0. Connect node 0 to both nodes 1 and 2.
Second node is labeled as 1. Connect node 1 to node 2.
Third node is labeled as 2. Connect node 2 to node 2 (itself), thus forming a self-cycle.
Visually, the graph looks like the following:
```
       1
      / \
     /   \
    0 --- 2
         / \
         \_/

```

题意：给一个图，复制这个图的内容


思路：

复制一个图，可以使用dfs也可以使用bfs，本质是遍历一个图
注意在遍历一个图的时候关键是标记一个节点有没有被遍历过，如果遍历过不再加入队列



```
/**
 * Definition for undirected graph.
 * struct UndirectedGraphNode {
 *     int label;
 *     vector<UndirectedGraphNode *> neighbors;
 *     UndirectedGraphNode(int x) : label(x) {};
 * };
 */
class Solution {
public:
//思路：复制一个图，可以使用dfs也可以使用bfs，本质是遍历一个图
//注意在遍历一个图的时候关键是标记一个节点有没有被遍历过，如果遍历过不再加入队列
    UndirectedGraphNode *cloneGraph(UndirectedGraphNode *node) {
        if(node==NULL) return NULL;
		unordered_map<int,UndirectedGraphNode *> record;
		queue<UndirectedGraphNode *> que;
		que.push(node);
		while(!que.empty())
		{
			UndirectedGraphNode *node=que.front();
			que.pop();
			UndirectedGraphNode *copynode;
			if(!record.count(node->label))
			{
				copynode=new UndirectedGraphNode(node->label);
				record[copynode->label]=copynode;
			}
			else
			{
				copynode=record[node->label];
			}
			for(int i=0;i<node->neighbors.size();i++)
			{
				UndirectedGraphNode *child;
				if(!record.count(node->neighbors[i]->label))
				{
					child=new UndirectedGraphNode(node->neighbors[i]->label);
					record[node->neighbors[i]->label]=child;
					que.push(node->neighbors[i]);
				}
				else
				{
					child=record[node->neighbors[i]->label];	
				}
				copynode->neighbors.push_back(child);
				
			}
		}
		return record[node->label];
    }
};
```
