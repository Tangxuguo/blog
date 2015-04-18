---
layout: post
title: "merge-sorted-array"
tags: [算法]
---	
	
>http://lintcode.com/en/problem/merge-sorted-array/
	
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
