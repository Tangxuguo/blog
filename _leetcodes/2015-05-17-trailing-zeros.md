---
layout: post
title: "trailing-zeros"
tags: [算法]
---	
	
>http://www.lintcode.com/en/problem/trailing-zeros/


考虑n!的质数因子。后缀0总是由质因子2和质因子5相乘得来的。如果我们可以计数2和5的个数，问题就解决了

我们很容易观察到质因子中2的个数总是大于等于5的个数


	class Solution {
	public:
	    // param n : description of n
	    // return: description of return
	    long long trailingZeros(long long n) {
	        long long counter = 0;
	        long long tmp = n;
	        while( tmp ) {
	            tmp = tmp/5;
	            counter += tmp;
	        };
	        return counter;
	    }
	};


参考资料：

+ http://bookshadow.com/weblog/2014/12/30/leetcode-factorial-trailing-zeroes/