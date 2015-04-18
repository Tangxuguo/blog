---
layout: post
title: "find-minimum-in-rotated-sorted-array"
tags: [算法]
---

> http://www.lintcode.com/en/problem/find-minimum-in-rotated-sorted-array/


解法一：循环遍历,复杂度 O(n)

	class Solution {
	public:
	    /**
	     * @param num: a rotated sorted array
	     * @return: the minimum number in the array
	     */
	    int findMin(vector<int> &num) {
	        // write your code here
	        int n = num.size();
	        if (n == 0) {
	            return 0;
	        }
	        if (num[0] <= num[num.size()-1]) {
	            return num[0];
	        }
	        int min = num[0];
	        for (int i = 0; i < n; i++) {
	            if (num[i] < min) {
	                return num[i];
	            }
	        }
	    }
	};

解法二：二分法，复杂度O(log2n)

	class Solution {
	public:
	    int findMin(vector<int>& nums) {
	        int start = 0;
	        int end = nums.size()-1;
	        int mid;
	        while (start+1 < end) {
	            mid =  start + (end - start)/2;
	            if (nums[start] < nums[end]) {
	                return nums[start];                
	            }
	            else if (nums[start] > nums[end]) {
	                if (nums[mid] >= nums[start] && nums[mid] >= nums[end]) {
	                    start = mid;   
	                } else { end = mid; }
	            }
	        }
	        if (nums[start] < nums[end]) {
	            return nums[start];
	        } else { return nums[end];} 
	    }
	};