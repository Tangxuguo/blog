---
layout: post
title: "matrix-zigzag-traversal"
tags: [算法]
---	
	
>http://www.lintcode.com/en/problem/matrix-zigzag-traversal/

状态转移方程，以及注意初始值

	class Solution {
	public:
	    /**
	     * @param n: An integer
	     * @return: An integer
	     */
	    int climbStairs(int n) {
	        // write your code here
	        vector<int> result(3, 0);
	        result[0] = 1;
	        result[1] = 2;
	        if (n == 0) return 0;
	        if (n == 1) return 1;
	        if (n == 2) return 2;
	        for (int i = 3; i <= n; i++) {
	            result[2] = result[0] +result[1];
	            result[0] = result[1];
	            result[1] = result[2];
	        }
	        return result[2];
	    }
	};


参考资料

+ http://blog.csdn.net/kenden23/article/details/17377869