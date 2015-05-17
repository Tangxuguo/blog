---
layout: post
title: "reverse-words-in-a-string"
tags: [算法]
---	
	
>http://www.lintcode.com/en/problem/reverse-words-in-a-string/

注意如何剔除空格，字符串反转函数reverse，以及如何插入空格

		class Solution {
	public:
	    /**
	     * @param s : A string
	     * @return : A string
	     */
	    string reverseWords(string s) {
	        // write your code here
	        string result;
	        for (int i = s.size() - 1; i >= 0; ) {
	            
	            while (i >= 0 && s[i] == ' ') i--;
	            if (i < 0) break;
	            
	            string tmp;
	            while (i >= 0 && s[i] != ' ') tmp.push_back(s[i--]);
	            reverse(tmp.begin(), tmp.end());
	            
	            if (!result.empty()) result.push_back(' ');
	            result.append(tmp); 
	            //result += tmp; 
	        }
	        return result;
	        
	    }
	};



参考 

+ http://blog.csdn.net/kenden23/article/details/20701069