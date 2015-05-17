---
layout: post
title: "first-bad-version"
tags: [算法]
---

>http://lintcode.com/en/problem/first-bad-version/

注意：题真的很简单


	/**
	 * class VersionControl {
	 *     public:
	 *     static bool isBadVersion(int k);
	 * }
	 * you can use VersionControl::isBadVersion(k) to judge wether 
	 * the kth code version is bad or not.
	*/
	class Solution {
	public:
	    /**
	     * @param n: An integers.
	     * @return: An integer which is the first bad version.
	     */
	    int findFirstBadVersion(int n) {
	        // write your code here
	        VersionControl v;
	        int start = 1;
	        int end = n;
	        int mid;
	        while (start+1 < end) {
	            mid =  start + (end - start)/2;
	            if (v.isBadVersion(mid)) {
	                end = mid;
	            }
	            else {
	                start = mid;
	            }
	        }
	        if (v.isBadVersion(start)) {
	            return start;
	        }
	        else {
	            return end;
	        }
	    }
	};
	
