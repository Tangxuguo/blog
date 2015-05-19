---
layout: post
title: "o1-check-power-of-2"
tags: [算法]
---	
	
>http://www.lintcode.com/en/problem/insert-node-in-a-binary-search-tree/

检查1的个数
注意边界条件
n可能非正，

	class Solution {
	public:
	    /*
	     * @param n: An integer
	     * @return: True or false
	     */
	    bool checkPowerOf2(int n) {
	        // write your code here
	        if ( n <= 0) {
	            return false;
	        }
	        int counter = 0;
	        while (n) {
	            n = n & (n - 1);
	            counter ++;
	        }
	        if (counter == 1) {
	            return true;
	        }
	        else {
	            return false;
	        }
	    }
	};
	
	
	  bool checkPowerOf2(int n) {
        if (1 > n) {
            return false;
        } else {
            return 0 == (n & (n - 1));
        }
    }
