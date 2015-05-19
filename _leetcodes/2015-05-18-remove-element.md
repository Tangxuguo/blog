---
layout: post
title: "remove-element"
tags: [算法]
---	
	
>http://www.lintcode.com/en/problem/remove-element/

用模板

	class Solution {
	public:
	    /** 
	     *@param A: A list of integers
	     *@param elem: An integer
	     *@return: The new length after remove
	     */
	    int removeElement(vector<int> &A, int elem) {
	        // write your code here
	        vector<int>::iterator it = A.begin();
	        while (it != A.end()) {
	            if (*it == elem) {
	                it = A.erase(it);
	            } else { it++;
	            }
	        }
	        return A.size();
	    }
	};

	
跳过

	class Solution {
	public:
	    /** 
	     *@param A: A list of integers
	     *@param elem: An integer
	     *@return: The new length after remove
	     */
	    int removeElement(vector<int> &A, int elem) {
	        // write your code here
	        int index = 0;
	        for (int i = 0; i < A.size(); i++) {
	            if(A[i] != elem) {
	                A[index++] = A[i];
	            }
	        }
	        return index;
	    }
	};


