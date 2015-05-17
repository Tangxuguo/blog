---
layout: post
title: "search-for-a-range"
tags: [算法]
---

>http://lintcode.com/en/problem/search-for-a-range/

注意和binarysearch一样的做法，找两次边界

	
	class Solution {
	    /** 
	     *@param A : an integer sorted array
	     *@param target :  an integer to be inserted
	     *return : a list of length 2, [index1, index2]
	     */
	public:
	    vector<int> searchRange(vector<int> &A, int target) {
	        // write your code here
	
	        vector<int> array = A;
	        vector<int>  result(2,-1);
	        if (array.size() == 0) {
	            return result;
	        }
	        int start = 0;
	        int end = array.size()-1;
	        int mid;
	        while (start+1 < end) {
	            mid = start + (end-start)/2;
	            if (array[mid] == target) {
	                end = mid;
	            }
	            else if (array[mid] > target) {
	                end = mid;
	            }
	            else {
	                start = mid;
	            }
	        }
	        if (array[start] == target) {
	            result[0] = start;
	        }
	        else if (array[end] == target) {
	            result[0] = end;
	        }
	        else {
	            result[0]=result[1]=-1;
	            return result;
	        }
	        start = 0;
	        end = array.size()-1;
	        while (start+1 < end) {
	            mid = start + (end-start)/2;
	            if (array[mid] == target) {
	                start = mid;
	            }
	            else if (array[mid] > target) {
	                end = mid;
	            }
	            else {
	                start = mid;
	            }
	        }
	        if (array[end] == target) {
	            result[1] = end;
	        }
	        else if (array[start] == target) {
	            result[1] = start;
	        }
	        else {
	            result[0] = result[1] = -1;
	            return result;
	        }
	        return result;
	    }
	};
