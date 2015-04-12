---
layout: post
title: "unique-subsets"
tags: [算法]
---

>http://lintcode.com/en/problem/unique-subsets/

函数默认给的是一个常量，不能对它sort，可以把const去掉，也可以把这个S赋给另外一个可以改变的s，然后排序

	class Solution {
	public:
	    /**
	     * @param S: A set of numbers.
	     * @return: A list of lists. All valid subsets.
	     */
	    vector<vector<int> > subsetsWithDup(const vector<int> &S) {
	        // write your code here
	        vector<vector<int>> result;
	        vector<int> path;
	        vector<int> s=S;
	        sort(s.begin(),s.end());
	        subsetsHelper(result,path,0,s);
	        return result;
	    }
	    void subsetsHelper(vector<vector<int> > &result, vector<int> &path, int pos, const vector<int> &S) {
	        result.push_back(path);
	        for(int i=pos; i<S.size(); i++){
	            if(i>0 && i!=pos && S[i]==S[i-1]){
	              continue;
	            }
	            path.push_back(S[i]);
	            subsetsHelper(result,path,i+1,S);
	            path.pop_back();
	          
	        }
	    }
	};

