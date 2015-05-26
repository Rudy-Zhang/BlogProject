title: "leetcode习题集 19 Remove Nth Node From End of List"
date: 2015-04-06 15:31:44
category: [算法]
tags: [leetcode习题集]
---

Given a linked list, remove the nth node from the end of list and return its head.

For example,

   Given linked list: 1->2->3->4->5, and n = 2.

   After removing the second node from the end, the linked list becomes 1->2->3->5.
Note:
Given n will always be valid.
Try to do this in one pass.

    For example, given array S = {-1 2 1 -4}, and target = 1.

    The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).


题意：删除链表中倒数第n个元素

思路：
需要两个指针，第一个指针先走n步，然后两个节点一起走，第一个走到空，第二个就指向倒数第n个

注意：
n<=0,n==1,n太大的情况

```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
//两个节点，第一个先走n步
//测试：n<0,n=0,n合适，n太大
//头结点和中间节点分开算
    ListNode *removeNthFromEnd(ListNode *head, int n) {
        if(n<=0) return head;
        ListNode *first=head,*second=head,*toBeDelete=head;
        for(int i=0;i<n;i++)
        {
            if(first==NULL) return head;
            first=first->next;
        }
        //假如要删除头结点
        if(first==NULL)
        {
            toBeDelete=head;
            head=head->next;
            delete toBeDelete;
            return head;
        }
        //假如要删除中间节点
        while(first->next)
        {
            first=first->next;
            second=second->next;
        }
        toBeDelete=second->next;
        second->next=toBeDelete->next;
        delete toBeDelete;
        return head;
        
    }
};
```