---
layout: post
title: "rotate-string"
tags: [算法]
---	
	
>http://www.lintcode.com/en/problem/rotate-string/

采用三步反转法。

以S="abcdefg"  offset=4为例子



首先将字符串看做："abc"+"defg"

先整体反转：得到 "gfed" + "cba"

然后各自反转：得到“defg” + "abc" = "defgabc"



时间复杂度O(n)，额外空间复杂度O(1)

注意边界判断，移位注意取余，注意大小为0情况

	class Solution {
	public:
	  /**
	     * param A: A string
	     * param offset: Rotate string with offset.
	     * return: Rotated string.
	     */
	    string rotateString(string A, int offset) {
	        // wirte your code here
	        if (A.size() == 0) {
	            return A;
	        }
	        offset = offset % A.size();
	        char tmp;
	        for (int i = 0; i < A.size()/2; i++) {
	            tmp = A[i];
	            A[i] = A[A.size() - i -1];
	            A[A.size() - i -1] = tmp;
	        }
	        for (int i = 0; i < offset/2; i++) {
	            tmp = A[i];
	            A[i] = A[offset - i -1];
	            A[offset - i -1] = tmp;
	        }
	        for (int i = offset; i < offset+(A.size()-offset)/2; i++) {
	            tmp = A[i];
	            A[i] = A[A.size() + offset - i -1];
	            A[A.size() + offset - i -1] = tmp;
	        }
	        return A;
	    }
	};


参考 http://www.jiuzhang.com/problem/55/