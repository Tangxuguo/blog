---
layout: post
title: "find-peak-element"
tags: [算法]
---

>http://lintcode.com/en/problem/find-peak-element/

注意：解法一，直接顺序检测，简单粗暴，但是耗时较长，没有注意


	class Solution {
	public:
	    /**
	     * @param A: An integers array.
	     * @return: return any of peek positions.
	     */
	    int findPeak(vector<int> A) {
	        // write your code here
	        for (int i = 1; i < A.size(); i++) {
	            if (A[i] > A [i-1] && A[i] > A[i+1]) {
	                return i;
	            }
	        }
	    }
	};

解法二：注意条件，没有重复元素，并且第二个比第一个大，倒数第二个比倒数第一个大，数学知识告诉我们这之间一定有一个峰值。二分法，如果中间元素大于其相邻后续元素，则中间元素左侧(包含该中间元素）必包含一个局部最大值。如果中间元素小于其相邻后续元素，则中间元素右侧必包含一个局部最大值,退出条件是当两者缩小到2，即刚好是一个峰值出现。

	class Solution {
	public:
	    /**
	     * @param A: An integers array.
	     * @return: return any of peek positions.
	     */
	    int findPeak(vector<int> A) {
	        // write your code here
	        int start = 0;
	        int end = A.size()-1;
	        int mid;
	        while (start+1 < end) {
	            mid = start + (end - start)/2;
	            if ((end - start) == 2) {
	                return start+1;
	            }
	            else if (A[mid] > A[mid+1]) {
	                end = mid +1;
	            }
	            else {
	                start = mid;
	            }
	        }
	    }
	};



	