---
layout: post
title: "search-in-rotated-sorted-array"
tags: [算法]
---

>http://lintcode.com/en/problem/search-in-rotated-sorted-array/
	
先给出一个O(n)的解法

	class Solution {
	    /** 
	     * param A : an integer ratated sorted array
	     * param target :  an integer to be searched
	     * return : an integer
	     */
	public:
	    int search(vector<int> &A, int target) {
	        // write your code here
	        int num = A.size();
	        for (int i = 0; i < num; i++) {
	            if (A[i] == target) {
	                return i;
	            }
	        }
	        return -1;
	    }
	};

第二种解法：二分法，分情况讨论

	class Solution {
	    /** 
	     * param A : an integer ratated sorted array
	     * param target :  an integer to be searched
	     * return : an integer
	     */
	public:
	    int search(vector<int> &A, int target) {
	        // write your code here
	        int start = 0;
	        int end = A.size()-1;
	        int mid;
	        if(A.size() == 0) {
	            return -1;
	        }
	        while (start + 1 <end) {
	            mid = start + (end -start)/2;
	            if (A[mid] == target) {
	                return mid;
	            }
	            else if (A[start] < A[mid]) {
	                if (A[start] <= target && target <= A[mid]) {
	                    end = mid;
	                }
	                else { start = mid; }
	            }
	            else if (A[start] > A[mid]) {
	                if (A[mid] <= target && target <= A[end]) {
	                    start = mid;
	                }            
	                else { end = mid; }
	            }
	                
	        }
	        
	        if (A[start] == target) {
	            return start;
	        }
	        if (A[end] ==  target) {
	            return end;
	        }
	        return -1;
	    }
	};

<img src="/blog/public/images/posts/code/sort-array.png" >

参考 http://fisherlei.blogspot.com/2013/01/leetcode-search-in-rotated-sorted-array.html
