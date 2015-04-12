---
layout: post
title: "letter-combinations-of-a-phone-number"
tags: [算法]
---

> https://leetcode.com/problems/letter-combinations-of-a-phone-number/


注意：开始初始化一个字母表，然后还是DFS和回溯，注意字符串的处理

	class Solution {
	public:
	    vector<string> letterCombinations(string digits) {
	        vector<string> result;
	        vector<string> digits_table (10);
	        string path;
	        if(digits.length()==0) {
	            return result;
	        }
	        init_digits_table(digits_table);
	        letterCombinationsHelper(result, path, 0, digits, digits_table);
	        return result;
	    }
	    void letterCombinationsHelper(vector<string> &result, string &path, int pos, string digits, vector<string> digits_table) {
	        if ( path.length() == digits.length() ) {
	            result.push_back(path);
	        }
	        for ( int i = pos; i<digits.length();i++) {
	            for(int j = 0; j<digits_table[digits[i]-'0'].length(); j++) {
	                path+=digits_table[digits[i]-'0'][j];
	                letterCombinationsHelper(result, path, i+1, digits, digits_table);
	                path=path.substr(0,path.length()-1);
	            }
	        }
	    }
	    void init_digits_table(vector<string> &digits_table) {
	        digits_table[1]="";
	        digits_table[2]="abc";
	        digits_table[3]="def";
	        digits_table[4]="ghi";
	        digits_table[5]="jkl";
	        digits_table[6]="mno";
	        digits_table[7]="pqrs";
	        digits_table[8]="tuv";
	        digits_table[9]="wxyz";
	        digits_table[0]="";
	        
	    }
	};