---
layout: post
title: "combination-sum-ii"
tags: [算法]
---

>https://leetcode.com/problems/combination-sum-ii/

DFS和回朔法



解法一：和subsets做法相同，控制退出条，插入时做判断是否重复，效率不高

	class Solution {
	public:
	    vector<vector<int> > combinationSum2(vector<int> &candidates, int target) {
	        vector<vector<int>> result;
	        vector<int> path;
	        sort(candidates.begin(), candidates.end());
	        combinationSumHelper(result, path, 0, candidates, target);
	        return result;
	    }
	    void combinationSumHelper(vector<vector<int>> &result, vector<int> &path, int pos, vector<int> &candidates, int target){
	        if(accumulate(path.begin(),path.end(), 0)== target){
	            for(int j=0; j<result.size(); j++){
	                if(result[j]==path){
	                    return;
	                }
	            }
	            result.push_back(path);
	            return;
	        }
	        if(accumulate(path.begin(),path.end(), 0)> target){
	            return;
	        }
	        for(int i = pos; i < candidates.size(); i++){
	            path.push_back(candidates[i]);
	            combinationSumHelper(result, path, i+1, candidates, target);
	            path.pop_back();
	        }
	    }
	};


解法二：和unique subsets做法类似，直接求出

	class Solution {
	public:
	    vector<vector<int> > combinationSum2(vector<int> &candidates, int target) {
	        vector<vector<int>> result;
	        vector<int> path;
	        sort(candidates.begin(), candidates.end());
	        combinationSumHelper(result, path, 0, candidates, target);
	        return result;
	    }
	    void combinationSumHelper(vector<vector<int>> &result, vector<int> &path, int pos, vector<int> &candidates, int target){
	        if( accumulate(path.begin(),path.end(), 0) == target ){
	            result.push_back(path);
	            return;
	        }
	        if(accumulate(path.begin(),path.end(), 0)> target){
	            return;
	        }
	        for(int i = pos; i < candidates.size(); i++){
	            if( i>0 && i != pos && candidates[i] == candidates[i-1]){
	                continue;
	            }
	            path.push_back(candidates[i]);
	            combinationSumHelper(result, path, i+1, candidates, target);
	            path.pop_back();
	        }
	    }
	};

      
   