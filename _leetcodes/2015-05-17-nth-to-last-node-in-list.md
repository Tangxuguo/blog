---
layout: post
title: "nth-to-last-node-in-list"
tags: [算法]
---	
	
>http://www.lintcode.com/en/problem/nth-to-last-node-in-list/

方法一：先找出长度，然后计算,注意head是第一个节点，所以length从1开始计数


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
	     * @return: Nth to last node of a singly linked list. 
	     */
	    ListNode *nthToLast(ListNode *head, int n) {
	        // write your code here
	         if (!head) return NULL;
	        ListNode * curr = head;
	        int length = 1;
	        while (curr -> next) {
	            curr = curr -> next;
	            ++length;
	        }
	        curr = head;
	        for (int i = 1; i < length - n +1; i++)
	            curr = curr -> next;
	        return curr;
	    }
	};

方法二：

两个指针：第一个指针先跑n后，第二个指针再跑，这样，第一个到末尾的时候，第二个刚好到nth






