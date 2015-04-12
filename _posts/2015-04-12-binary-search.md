---
layout: post
title: "binary-search"
tags: [算法]
---

> http://www.lintcode.com/en/problem/binary-search/


Keypoints:1. start + 1 < end2. start + (end - start) / 2 3. A[mid] ==, <, >4. A[start] A[end] ? target

二分查找：递归版本,原来的函数不适合递归，重新写一个，注意要寻找最前面的一个

	class Solution {
	public:
	    /**
	     * @param nums: The integer array.
	     * @param target: Target number to find.
	     * @return: The first position of target. Position starts from 0. 
	     */
	    int binarySearch(vector<int> &array, int target) {
	        // write your code here
	        int start = 0;
	        int end = array.size()-1;
	        int result;
	        result = binarySearchHelper(array, start, end, target);
	        return result;
	    }
	    int binarySearchHelper(vector<int> &array, int &start, int &end, int target) {
	        int mid ;
	        if(start+1<end) {
	            mid = start + (end-start)/2;
	            if ( array[mid] == target) {
	                end = mid ;
	            }
	            else if( array[mid] > target ) {
	                end = mid;
	            }
	            else {
	                start = mid;
	            }
	            binarySearchHelper(array, start, end, target);
	        }
	        
	        if (array[start] == target) {
	            return start;
	        }
	        else if (array[end] == target) {
	            return end;
	        }
	        else 
	            return -1;
	        
	        
	    }
	};

非递归版本：

	class Solution {
	public:
	    /**
	     * @param nums: The integer array.
	     * @param target: Target number to find.
	     * @return: The first position of target. Position starts from 0. 
	     */
	    int binarySearch(vector<int> &array, int target) {
	        // write your code here
	        if(array.size()==0) {
	            return -1;
	        }
	        int start = 0;
	        int end = array.size()-1;
	        int mid ;
	        while(start+1<end) {
	            mid = start + (end-start)/2;
	            if ( array[mid] == target) {
	                end = mid ;
	            }
	            else if( array[mid] > target ) {
	                end = mid;
	            }
	            else {
	                start = mid;
	            }
	        }
	        
	        if (array[start] == target) {
	            return start;
	        }
	        else if (array[end] == target) {
	            return end;
	        }
	        return -1;
	    }
	};

传统的二分法不适合：只能找到相等的，不能找到最前面的一个，需要改

	int binarySearch(int a[],int n,int key){
	    int low=0,high=n-1;
	    int mid;
	    while(low<=high){
	        mid=low+(high-low)/2;
	        if(key<a[mid])
	            high=mid-1;
	        else if(key>a[mid])
	            low=mid+1;
	        else
	            return mid;
	    }
	    return -(low+1);
	}
