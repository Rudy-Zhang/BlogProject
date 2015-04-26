title: "leetcode习题集 2 Add Two Numbers"
date: 2015-04-06 15:31:44
category: [leetcode习题集]
tags: []
---

You are given two linked lists representing two non-negative numbers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8

题意：两个链表加和

思路：
链表顺序和加和顺序一样，否则需要翻转链表。
1. 重新建立一个链表，每次new出节点，加和
2. 使用原来的比较长的链表，加到这个链表上，最多new一个节点

注意：
头结点手动建立，再往后遍历
链表长度不一样的处理


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
    ListNode *addTwoNumbers(ListNode *l1, ListNode *l2) {
        ListNode *n1=l1,*n2=l2,*n=NULL;
        if(n1==NULL) return l2;
        if(n2==NULL) return l1;
        ListNode *l3=NULL;
        
        int temp=(n1->val+n2->val)%10;
        int carry=(n1->val+n2->val)/10;
        n1=n1->next;
        n2=n2->next;
        l3=new ListNode(temp);
        n=l3;
        //这里改为或可以减少很多判断
        while(n1!=NULL && n2!=NULL)
        {
            temp=(n1->val+n2->val+carry)%10;
            carry=(n1->val+n2->val+carry)/10;
            ListNode *tnode=new ListNode(temp);
            n->next=tnode;
            n=n->next;
            n1=n1->next;
            n2=n2->next;
        }
        if(carry==0)
        {
            if(n1==NULL)
            {
                n->next=n2;
                return l3;
            }
            else if(n2==NULL)
            {
                n->next=n1;
                return l3;
            }
        }
        else
        {
            if(n1==NULL)
            {
                while(n2!=NULL)
                {
                    temp=(n2->val+carry)%10;
                    carry=(n2->val+carry)/10;
                    ListNode *tnode=new ListNode(temp);
                    n->next=tnode;
                    n=n->next;
                    n2=n2->next;
                }
            }
            else if(n2==NULL)
            {
                while(n1!=NULL)
                {
                    temp=(n1->val+carry)%10;
                    carry=(n1->val+carry)/10;
                    ListNode *tnode=new ListNode(temp);
                    n->next=tnode;
                    n=n->next;
                    n1=n1->next;
                }
            }
            if(carry==0) return l3;
            else
            {
                ListNode *tnode=new ListNode(carry);
                n->next=tnode;
                n=n->next;
                return l3;
            }
        }
    }
};
```