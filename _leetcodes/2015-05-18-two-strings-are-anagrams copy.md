---
layout: post
title: "two-strings-are-anagrams"
tags: [算法]
---	
	
>http://www.lintcode.com/en/problem/two-strings-are-anagrams/


先排序，再比较O(nlogn)，比较郁闷的时，说的是颠倒顺序，我还特意把不变顺序的拿出来来的，结果错了

	class Solution {
	public:
	    /**
	     * @param s: The first string
	     * @param b: The second string
	     * @return true or false
	     */
	    bool anagram(string s, string t) {
	        // write your code here
	
	        sort(s.begin(), s.end());
	        sort(t.begin(), t.end());
	        if(s == t ) {
	            return true;
	        }
	        else {
	            return false;
	        }
	    }
	};

HASH统计O(n),或者建一个表，扫描一遍，统计个字母出现的次数，最后再比较一次

这里用map的方法

	class Solution {
	public:
	    /**
	     * @param s: The first string
	     * @param b: The second string
	     * @return true or false
	     */
	    bool anagram(string s, string t) {
	        // write your code here
	        map<char, int> lettersInWord1;
	        map<char, int>::iterator iter;
	        for(char c : s) {
	            if(lettersInWord1.find(c) == lettersInWord1.end()) {
	                lettersInWord1.insert(pair <char, int>  ( c, 0 ));
	            }
	            lettersInWord1[c]++;
	        }
	        for(char c : t) {
	            if(lettersInWord1.find(c) == lettersInWord1.end()) {
	                lettersInWord1.insert(pair <char, int>  ( c, 0 ));
	            }
	            lettersInWord1[c]--;
	        }
	        for ( iter = lettersInWord1.begin( ); iter != lettersInWord1.end( ); iter++ ) {
	            if(iter -> second != 0){
	                return false;
	            }
	        }
	        return true;
	    }
	};
