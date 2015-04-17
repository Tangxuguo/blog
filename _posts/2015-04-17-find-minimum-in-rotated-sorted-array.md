---
layout: post
title: "find-minimum-in-rotated-sorted-array"
tags: [算法]
---

> http://www.lintcode.com/en/problem/find-minimum-in-rotated-sorted-array/


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
