---
layout: post
title: "remove-duplicates-from-sorted-array-ii"
tags: [算法]
---

> http://lintcode.com/en/problem/remove-duplicates-from-sorted-array-ii/	
	
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
	        int counter = 0;
	        for (int i = 1; i < nums.size(); i++) {
	            if (nums[i] == nums[i-1]) {
	                counter++;
	                if (counter>=2) {
	                nums.erase(nums.begin() + i);
	                i--;
	                }
	            }
	            else {
	            counter = 0;
	            }
	        }
	        return nums.size();
	    }
	};
