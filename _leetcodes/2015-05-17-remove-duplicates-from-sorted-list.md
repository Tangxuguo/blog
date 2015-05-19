---
layout: post
title: "remove-duplicates-from-sorted-list"
tags: [算法]
---	
	
>http://www.lintcode.com/en/problem/remove-duplicates-from-sorted-list/

方法一：双指针，注意释放内存


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
	     * @return: head node
	     */
	    ListNode *deleteDuplicates(ListNode *head) {
	        // write your code here
	        if (!head) return NULL;
	        ListNode *curr = head -> next;
	        ListNode *prev = head;
	        while (curr) {
	            if(!curr) return head;
	            if(curr -> val == prev -> val) {
	                ListNode* temp = curr;
	                curr = curr -> next;
	                prev->next = curr;
	                delete temp;
	                continue;
	            }
	            prev = curr;
	            curr = curr ->next;
	        }
	        return head;
	    }
	};
