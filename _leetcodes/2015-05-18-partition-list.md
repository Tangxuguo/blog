---
layout: post
title: "partition-list"
tags: [算法]
---	
	
>http://www.lintcode.com/en/problem/partition-list/


先创建两个链表，分别存比x小的链表和比x大的链表，最后吧两个链表链接起来, 返回的时候记得删除头结点，避免内存泄露
创建结点：

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
	     * @param x: an integer
	     * @return: a ListNode 
	     */
	    ListNode *partition(ListNode *head, int x) {
	        // write your code here
	        ListNode * head1 = new ListNode();
	        ListNode * p1 = head1;
	        ListNode * head2 = new ListNode();;
	        ListNode * p2 = head2;
	        ListNode * curr = head;
	        while (curr != NULL) {
	            if (curr->val < x) {
	                p1->next = new ListNode(curr->val);
	                p1 = p1->next;
	            } else {
	                p2->next = new ListNode(curr->val);
	                p2 = p2->next;
	            }
	            curr = curr -> next;
	        }
	        if (head2->next!=NULL) p1->next=head2->next;
	        delete head2;
	        return head1->next;
	    }
	};
	
不创建结点，记得插完结点后把next置空

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
	    * @param x: an integer
	    * @return: a ListNode 
	    */
	   ListNode *partition(ListNode *head, int x) {
	       // write your code here
	       if(head == NULL)  
	       return NULL;
	       ListNode * head1 = new ListNode();
	       ListNode * p1 = head1;
	       ListNode * head2 = new ListNode();;
	       ListNode * p2 = head2;
	       ListNode * curr = head;
	       while (curr != NULL) {
	           ListNode *next = curr->next;
	           if (curr->val < x) {
	               p1->next = curr;
	               p1 = p1->next;
	           } else {
	               p2->next = curr;
	               p2 = p2->next;
	           } 
	           curr ->next = NULL; //  important
	           curr = next;
	       }
	       if (head2->next!=NULL) p1->next=head2->next;
	       delete head2;
	       ListNode *tmp = head1->next;
	       delete head1;
	       return tmp;
	   }
	};







参考资料

+ http://fisherlei.blogspot.com/2013/11/leetcode-single-number-ii-solution.html
+ http://blog.csdn.net/kenden23/article/details/13625297