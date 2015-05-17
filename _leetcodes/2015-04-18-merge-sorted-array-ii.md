---
layout: post
title: "merge-sorted-array-ii"
tags: [算法]
---	
	
>http://lintcode.com/en/problem/merge-sorted-array-ii/
	
	class Solution {
	public:
	    /**
	     * @param A and B: sorted integer array A and B.
	     * @return: A new sorted integer array
	     */
	    vector<int> mergeSortedArray(vector<int> &A, vector<int> &B) {
	        // write your code here
	        int sizeA = A.size();
	        int sizeB = B.size();
	        vector<int> result;
	        if (A.empty()) {
	            result = B;
	            return result;
	        }
	        if(B.empty()) {
	            result = A;
	            return result;
	        }
	        int i = 0;
	        int j = 0;
	        while(i < sizeA && j < sizeB) {
	            if (A[i] < B[j]) {
	                result.push_back(A[i++]);
	            }
	            else if (A[i] > B[j]) {
	                result.push_back(B[j++]);
	            }
	            else {
	                result.push_back(A[i++]);
	                result.push_back(B[j++]);
	            }
	        }
	        if(i == sizeA) {
	            for(; j < sizeB; ++j) {
	                result.push_back(B[j]);
	            }
	        }
	        if( j == sizeB) {
	            for (;i < sizeA;++i) {
	                result.push_back(A[i]);
	            }
	        }
	        return result;
	    }
	};
	
	
	
	class Solution {
	public:
	    /**
	     * @param A and B: sorted integer array A and B.
	     * @return: A new sorted integer array
	     */
	    vector<int> mergeSortedArray(vector<int> &A, vector<int> &B) {
	        // write your code here
	        int m = 0;
	        int n = 0;
	        vector<int> result;
	        while (m < A.size() && n < B.size()) {
	            if (A[m] <= B[n]) {
	                result.push_back(A[m++]);
	            }
	            else {
	                result.push_back(B[n++]);
	            }
	        }
	        while (m < A.size()) {
	            result.push_back(A[m++]);
	        }
	        while (n < B.size()) {
	            result.push_back(B[n++]);
	        }
	        return result;
	    }
	};
	
	
	
