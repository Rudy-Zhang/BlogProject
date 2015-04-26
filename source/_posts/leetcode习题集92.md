title: "leetcode习题集 92 Reverse Linked List II "
date: 2015-04-06 15:31:44
category: [leetcode习题集]
tags: []
---
Reverse a linked list from position m to n. Do it in-place and in one-pass.

For example:
Given `1->2->3->4->5->NULL`, m = 2 and n = 4,

return `1->4->3->2->5->NULL`.

Note:
Given m, n satisfy the following condition:
1 ≤ m ≤ n ≤ length of list.

题意：反转一个链表的指定部分

思路：

1. 将原始链表分为三个部分part1,2,3翻转part2部分到新链表然后连接起来
2. p1,p2在reverse部分的前一个节点和最后一个节点停下，翻转，注意考虑前后为空的处理

注意：
可以相信mn的值，但是也要判断一下，头结点和空节点的处理

```
class Solution {
public:
//思路：将原始链表分为三个部分part1,2,3翻转part2部分到新链表然后连接起来
//注意：可以相信mn的值，但是也要判断一下，头结点和空节点的处理
//思路2：p1,p2在reverse部分的前一个节点和最后一个节点停下，翻转，注意考虑前后为空的处理
    ListNode *reverseBetween(ListNode *head, int m, int n) {
        ListNode *part1head=head,*node=head;
        ListNode *part1tail=NULL,*part2head=NULL,*part2tail=NULL,*part3head=NULL;
        if(m==n) return head;
        if(m==1)
        {
            part1head=part1tail=NULL;
			part2head=head;
        }
		else
		{
		    //判断的时候出现了错误，应该从1开始到m-1
			for(int i=1;i<m-1&&node->next!=NULL;i++)
				node=node->next;
			part1tail=node;
			node=node->next;
			part2head=node;
		}
        
        for(int i=0;i<n-m&&node->next!=NULL;i++)
            node=node->next;
        part2tail=node;
        if(node->next==NULL)
            part3head=NULL;
        else
        {
            part3head=node->next;
            node->next=NULL;
        }
        ListNode *revhead=NULL,*revtail=NULL;
        node=part2head;
        ListNode *tnode=node->next;
        node->next=revhead;
        revhead=node;
        revtail=revhead;
        node=tnode;
        while(node)
        {
            tnode=node->next;
            node->next=revhead;
            revhead=node;
            node=tnode;
        }
        if(part1head)
        {
            part1tail->next=revhead;
            revtail->next=part3head;
            return part1head;
        }
        else
        {
            revtail->next=part3head;
            return revhead;
        }
        
    }
};
```
