---
layout: post
title: "unique-characters"
tags: [算法]
---	
	
>http://www.lintcode.com/en/problem/unique-characters/

直接所有遍历一遍

时间复杂度O(n^2)

	class Solution {
	public:
	    /**
	     * @param str: a string
	     * @return: a boolean
	     */
	    bool isUnique(string &str) {
	        // write your code here
	        for( int i = 0; i < str.size(); i++) {
	            for (int j = i + 1; j < str.size(); j++) {
	                if (str[i] == str[j]) {
	                    return false;
	                }
	            }
	        }
	        return true;
	        
	    }
	};


快排+扫描

时间复杂度O(nlog(n)+n)
Time Complexity: O(nlogn)
Space Complexity: O(n)

bitmap
每个字母存一个标志位 ASCII 256位
Time Complexity: O(n)
Space Complexity: O(1)

	class Solution {
	public:
	    /**
	     * @param str: a string
	     * @return: a boolean
	     */
	    bool isUnique(string &str) {
	        // write your code here
	        vector<bool> tmp(256, false);
	        for (int i = 0; i< str.size(); i++) {
	            if (tmp[ str[i] ] == true) {
	                return false;
	            }
	            tmp[ str[i] ] = true;
	        }
	        return true;
	    }
	};


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


参考资料

+ http://www.jyuan92.com/blog/careercup1_1-unique-characters/