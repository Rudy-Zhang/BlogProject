title: "leetcode习题集 83 Remove Duplicates from Sorted List"
date: 2015-04-06 15:31:44
category: [算法]
tags: [leetcode习题集]
---
Given a sorted linked list, delete all duplicates such that each element appear only once.

For example,
Given `1->1->2`, return `1->2`.
Given `1->1->2->3->3`, return `1->2->3`.

题意：给一个链表，删除链表中的重复元素

思路：两个指针，遍历，模拟


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
//通过两个指针同时向前移动删除元素
//注意：为空，一个元素，多个元素，尾元素
    ListNode *deleteDuplicates(ListNode *head) {
        if(NULL == head || NULL==head->next)
            return head;
        ListNode *n1=head;
        ListNode *n2=head->next;
        while(n2)
        {
            if(n1->val == n2->val)
            {
                ListNode *toBeDelete;
                toBeDelete=n2;
                n1->next=n2->next;
                n2=n1->next;
                delete toBeDelete;
            }
            else
            {
                n1=n1->next;
                n2=n2->next;
            }
        }
        return head;
    }
};
```
