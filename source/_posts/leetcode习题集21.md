title: "leetcode习题集 21 Merge Two Sorted Lists"
date: 2015-04-06 15:31:44
category: [算法]
tags: [leetcode习题集]
---

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.


题意：合并两个已经排好序的链表为一个新的链表，不能使用新的存储空间

思路：
就一个一个合并

注意：
链表为空，头结点处理

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
//假如至少有一个为空，返回另一个
//两个都不是空
    ListNode *mergeTwoLists(ListNode *l1, ListNode *l2) {
        if(l1==NULL) return l2;
        if(l2==NULL) return l1;
        ListNode *l3;
        ListNode *p1=l1,*p2=l2,*p3=NULL;
        if(p1->val <= p2->val)
        {
            l3=p1;
            p1=p1->next;
            l3->next=NULL;
            p3=l3;
        }
        else
        {
            l3=p2;
            p2=p2->next;
            l3->next=NULL;
            p3=l3;
        }
        while(p1!=NULL && p2!=NULL)
        {
            if(p1->val <= p2->val)
            {
                p3->next=p1;
                p1=p1->next;
                p3=p3->next;
                p3->next=NULL;
            }
            else
            {
                p3->next=p2;
                p2=p2->next;
                p3=p3->next;
                p3->next=NULL;
            }
        }
        if(p1) p3->next=p1;
        else if(p2) p3->next=p2;
        return l3;
    }
};
```