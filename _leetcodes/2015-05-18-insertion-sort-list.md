---
layout: post
title: "insertion-sort-list"
tags: [算法]
---	
	
>http://www.lintcode.com/en/problem/insertion-sort-list/

注意:通过添加一个头结点，避免头部插入，另外插入的话要考虑前后的指针

>用一个辅助指针来做表头避免处理改变head的时候的边界情况

	/**
	 * Definition of ListNode
	 * class ListNode {
	 * public:
	 *     int val;
	 *     ListNode *next;
	 *     ListNode(int val) {
	 *         this->val = val;
	 *         this->next = NULL;
	 *     }
	 * }
	 */
	class Solution {
	public:
	    /**
	     * @param head: The first node of linked list.
	     * @return: The head of linked list.
	     */
	    ListNode *insertionSortList(ListNode *head) {
	        // write your code here
	        if (head == NULL) return NULL;
	        ListNode *result = new ListNode();
	        ListNode *p = result;
	        ListNode *curr = head;
	        while (curr != NULL) {
	            ListNode *next = curr->next;
	            p = result;
	            while (p->next != NULL && p->next->val <= curr -> val) {
	                p = p->next; //移到插入位置
	            }
	            curr->next = p->next//将当前结点的next指向要插入的下一个
	            p->next = curr;
	            curr = next;
	        }
	        return result->next;
	    }
	};



参考资料

+ http://blog.csdn.net/linhuanmars/article/details/21144553