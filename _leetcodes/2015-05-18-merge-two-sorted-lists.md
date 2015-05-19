---
layout: post
title: "merge-two-sorted-lists"
tags: [算法]
---	
	
>http://www.lintcode.com/en/problem/merge-two-sorted-lists/

链表不带头结点,坑,注意指针递归

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
	     * @param ListNode l1 is the head of the linked list
	     * @param ListNode l2 is the head of the linked list
	     * @return: ListNode head of linked list
	     */
	    ListNode *mergeTwoLists(ListNode *l1, ListNode *l2) {
	        // write your code here
	        
	        if(l1 == NULL) return l2;
	        if(l2 == NULL) return l1;
	        ListNode * result = new ListNode();
	        ListNode * p = result;
	        ListNode * tmp1 = l1;
	        ListNode * tmp2 = l2;
	        
	        while(tmp1 != NULL && tmp2 != NULL) {
	            if(tmp1->val > tmp2->val) {
	               p->next = new ListNode(tmp2->val);
	               tmp2 = tmp2->next;
	            }
	            else {
	               p->next = new ListNode(tmp1->val);
	               tmp1 = tmp1->next;
	            }
	            p = p->next;
	        }
	        while(tmp1 != NULL) {
	            p->next = new ListNode(tmp1->val);
	            tmp1 = tmp1->next;
	            p = p->next;
	        }
	        while(tmp2 != NULL) {
	            p->next = new ListNode(tmp2->val);
	            tmp2 = tmp2->next;
	            p = p->next;
	        }
	        return result->next;
	    }
	};

不新建链表

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
	     * @param ListNode l1 is the head of the linked list
	     * @param ListNode l2 is the head of the linked list
	     * @return: ListNode head of linked list
	     */
	    ListNode *mergeTwoLists(ListNode *l1, ListNode *l2) {
	        // write your code here
	        
	        if(l1 == NULL) return l2;
	        if(l2 == NULL) return l1;
	        ListNode * result = new ListNode();
	        ListNode * p = result;
	        ListNode * tmp1 = l1;
	        ListNode * tmp2 = l2;
	        
	        while(tmp1 != NULL && tmp2 != NULL) {
	            if(tmp1->val > tmp2->val) {
	               p->next = tmp2;
	               tmp2 = tmp2->next;
	            }
	            else {
	               p->next = tmp1;
	               tmp1 = tmp1->next;
	            }
	            p = p->next;
	        }
	        if(tmp1 != NULL) {
	            p->next = tmp1;
	        }
	        if(tmp2 != NULL) {
	            p->next = tmp2;
	        }
	        return result->next;
	    }
	};

或操作

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
	     * @param ListNode l1 is the head of the linked list
	     * @param ListNode l2 is the head of the linked list
	     * @return: ListNode head of linked list
	     */
	    ListNode *mergeTwoLists(ListNode *l1, ListNode *l2) {
	        // write your code here
	        
	        if(l1 == NULL) return l2;
	        if(l2 == NULL) return l1;
	        ListNode * result = new ListNode();
	        ListNode * p = result;
	        ListNode * tmp1 = l1;
	        ListNode * tmp2 = l2;
	        
	        while(tmp1 != NULL || tmp2 != NULL) {
	            
	            if(tmp1 == NULL) {
	                p->next = tmp2;
	                break;
	            }
	            if(tmp2 == NULL) {
	                p->next = tmp1;
	                break;
	            }
	            if(tmp1->val > tmp2->val) {
	               p->next = tmp2;
	               tmp2 = tmp2->next;
	            }
	            else {
	               p->next = tmp1;
	               tmp1 = tmp1->next;
	            }
	            p = p->next;
	        }
	        return result->next;
	    }
	};
	
