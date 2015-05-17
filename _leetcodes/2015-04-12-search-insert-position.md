---
layout: post
title: "search-insert-position"
tags: [算法]
---


>http://lintcode.com/en/problem/search-insert-position/

注意：插入的时候三种情况分类讨论，vector插入用法

	class Solution {
	    /**
	     * param A : an integer sorted array
	     * param target :  an integer to be inserted
	     * return : an integer
	     */
	public:
	    int searchInsert(vector<int> &A, int target) {
	        // write your code here
	        if (A.size() == 0) {
	            A.insert(A.begin(),target);
	            return 0;
	        }
	        int start = 0;
	        int end = A.size()-1;
	        int mid;
	        while (start+1 < end) {
	            mid = start + (end-start)/2;
	            if (A[mid] == target) {
	                end = mid;
	            }
	            else if (A[mid] > target) {
	                end = mid;
	            }
	            else {
	                start = mid;
	            }
	        }
	        if (A[start] == target) {
	            return start;
	        }
	        else if (A[end] == target) {
	            return  end;
	        }
	        else if (A[start] > target) {
	            A.insert(A.begin()+start,target);
	            return  start;
	        }
	        else if (A[end] < target) {
	            A.insert(A.end(),target);
	            end = end+1;
	            return end;
	        }
	        else {
	            A.insert(A.begin()+start+1,target);
	            start=start+1;
	            return start;
	        }
	    }
	};