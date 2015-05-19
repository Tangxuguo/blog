---
layout: post
title: "remove-nth-node-from-end-of-list"
tags: [算法]
---	
	
>http://www.lintcode.com/en/problem/remove-nth-node-from-end-of-list/

题目条件是n不大于length


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
	     * @param n: An integer.
	     * @return: The head of linked list.
	     */
	    ListNode *removeNthFromEnd(ListNode *head, int n) {
	        // write your code here
	        if (head == NULL) return head;
	        ListNode * pNode = head;
	        int length = 1;
	        while (pNode -> next != NULL) {
	            pNode = pNode -> next;
	            length++;
	        }
	        pNode = head;
	        if (length == n) {
	            ListNode *tmp = head -> next;
	            delete head;
	            return tmp;
	        }
	        for (int i = 1; i < length-n; i++) {
	               pNode = pNode -> next;
	        }
	        ListNode *tmp = pNode -> next;
	        pNode -> next = pNode -> next -> next;
	        delete tmp;
	        return head;
	    }
	};

双指针法，但是也要排除头指针的情况，前面的指针多移一个，后面的少移一个

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
	     * @param n: An integer.
	     * @return: The head of linked list.
	     */
	    ListNode *removeNthFromEnd(ListNode *head, int n) {
	        // write your code here
	        if (head == NULL) return head;
	        int length = 1;
	        
	        ListNode * prev = head;
	        ListNode * curr = head;
	        while (curr -> next !=NULL) {
	            curr = curr -> next;
	            length++;
	        }
	        if (length == n) {
	            ListNode *tmp = head -> next;
	            delete head;
	            return tmp;
	        }
	        
	        curr = head;
	        for (int i = 1; i < n + 1; i++) {
	             prev = prev -> next;
	        }
	        
	        while (prev -> next != NULL) {
	            prev = prev -> next;
	            curr = curr -> next;
	        }
	        
	        ListNode *tmp = curr -> next;
	        curr -> next = curr -> next -> next;
	        delete tmp;
	        return head;
	    }
	};


