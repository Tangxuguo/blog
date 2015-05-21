---
layout: post
title: "single-number"
tags: [算法]
---	
	
>http://www.lintcode.com/en/problem/single-number/

直接所有遍历一遍

时间复杂度O(n^2)



快排+扫描

时间复杂度O(nlog(n)+n)
Time Complexity: O(nlogn)
Space Complexity: O(n)

bitmap
每个数字存一个标志位
Time Complexity: O(n)
Space Complexity: O(1)

HASHmap，统计每个字母出现的频率，或者set
Time Complexity: O(n)
Space Complexity: O(n)  (这应该也是O(1)吧)

	class Solution {
	public:
	    /**
	     * @param str: a string
	     * @return: a boolean
	     */
	    bool isUnique(string &str) {
	        // write your code here
	        set<char> tmp;
	        for (int i = 0; i < str.size(); i++) {
	            if(tmp.find(str[i]) == tmp.end()) {
	                tmp.insert(str[i]);
	            }
	            else {
	                return false;
	            }
	        }
	        return true;
	    }
	};

上面的都不可靠：

异或：
条件是必须偶数个，如果出现奇数个失效；
异或操作，相同为0不同为1，所以任何与0异或等于本身。与自身相与为0

	class Solution {
	public:
		/**
		 * @param A: Array of integers.
		 * return: The single number.
		 */
	    int singleNumber(vector<int> &A) {
	        // write your code here
	        int result = 0;
	        for (int i = 0; i < A.size(); i++) {
	            result ^=A[i];
	        }
	        return result;
	    }
	};



参考资料

+ http://www.jyuan92.com/blog/careercup1_1-unique-characters/