---
layout: post
title: "merge-sorted-array"
tags: [算法]
---	
	
	
> http://lintcode.com/en/problem/merge-sorted-array/


插入排序
	
	class Solution {
	public:
	    /**
	     * @param A: sorted integer array A which has m elements, 
	     *           but size of A is m+n
	     * @param B: sorted integer array B which has n elements
	     * @return: void
	     */
	    void mergeSortedArray(int A[], int m, int B[], int n) {
	        // write your code here
	        int i, j, tmp;
	        for ( i = 0; i < n; i++ ) {
	            tmp = B[i];
	            for ( j = m+i; j > 0 && A[j-1] > tmp; j--) {
	                A[j] = A[j-1];
	            }
	            A[j] = tmp;
	        }
	    }
	};

