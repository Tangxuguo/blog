---
layout: post
title: "reverse-linked-list"
tags: [算法]
---	
	
>http://www.lintcode.com/en/problem/reverse-linked-list/

分别用连个指针记录当前节点的前一个和后一个节点


	/**
	 * Definition of ListNode
	 * 
	 * class ListNode {
	 * public:
	 *     int val;
	 *     ListNode *next;
	 * 
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
	     * @return: The new head of reversed linked list.
	     */
	    ListNode *reverse(ListNode *head) {
	        // write your code here
	        if (!head) return NULL;
	        ListNode * curr = head;
	        ListNode * prev;
	        ListNode * post;
	        post = curr -> next; 
	        head -> next = NULL;
	        curr = post;
	        prev = head;
	        while(curr) {
	            post = curr -> next;
	            curr -> next = prev;
	            prev = curr;
	            curr = post;
	        }
	        return prev;
	    }
	};




