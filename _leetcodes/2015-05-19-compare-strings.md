---
layout: post
title: "compare-strings"
tags: [算法]
---	
	
>http://www.lintcode.com/en/problem/compare-strings/

要注意的时题目的意思是每个字符都要有，考虑个数

	class Solution {
	public:
	    /**
	     * @param A: A string includes Upper Case letters
	     * @param B: A string includes Upper Case letter
	     * @return:  if string A contains all of the characters in B return true 
	     *           else return false
	     */
	    bool compareStrings(string A, string B) {
	        // write your code here
	        vector<int> counter(256, 0);
	        for (int i = 0; i < A.size(); i++) {
	            counter[ A[i] ] ++;
	        }
	        for (int i = 0; i < B.size(); i++) {
	            counter[ B[i] ] --;
	        }
	        for (int i = 0; i < 256; i++) {
	            if (counter[i] < 0)
	                return false;
	        }
	        return true;
	    }
	};



参考资料

+ http://blog.csdn.net/kenden23/article/details/17377869