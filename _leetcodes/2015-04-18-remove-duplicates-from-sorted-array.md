---
layout: post
title: "remove-duplicates-from-sorted-array"
tags: [算法]
---

> http://lintcode.com/en/problem/remove-duplicates-from-sorted-array/



	class Solution {
	public:
	    /**
	     * @param A: a list of integers
	     * @return : return an integer
	     */
	    int removeDuplicates(vector<int> &nums) {
	        // write your code here
	        int n = nums.size();
	        if (n <=1) {
	            return n;
	        }
	        for (int i = 1; i < nums.size(); i++) {
	            if (nums[i] == nums[i-1]) {
	                nums.erase(nums.begin() + i);
	                i--;
	            }
	        }
	        return nums.size();
	    }
	};
	
