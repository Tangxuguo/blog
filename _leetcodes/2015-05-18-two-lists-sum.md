---
layout: post
title: "two-lists-sum"
tags: [算法]
---	
	
>http://www.lintcode.com/en/problem/two-lists-sum/

链表不带头结点

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
	    /**
	     * @param l1: the first list
	     * @param l2: the second list
	     * @return: the sum list of l1 and l2 
	     */
	    ListNode *addLists(ListNode *l1, ListNode *l2) {
	        // write your code here
	        ListNode * result = new ListNode(0);
	        ListNode * tmp = result;
	        int carry = 0;
	        int val1, val2;
	        
	        while((l1 != NULL) || (l2 != NULL) || (carry != 0)) {
	            val1 = (l1 == NULL) ? 0 : l1->val;
	            val2 = (l2 == NULL) ? 0 : l2->val;
	            
	            tmp -> val = (val1 + val2 + carry);
	            carry = (val1 + val2 + carry)/10;
	            if (l1 != NULL) l1 = l1 -> next;
	            if (l2 != NULL) l2 = l2 -> next;
	            
	            if((l1 == NULL) && (l2 == NULL) && (carry == 0)) {
	                return result;
	            }
	            
	            tmp -> next = new ListNode(0);
	            tmp = tmp -> next;
	        }
	        return result;
	    }
	};
