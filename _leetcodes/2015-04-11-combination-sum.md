---
layout: post
title: "combination-sum"
tags: [算法]
---

>https://leetcode.com/problems/combination-sum/

DFS和回朔法

和subsets做法相同，求子集的时候是可以重复的，控制退出条件，避免耗时太长

	class Solution {
	public:
	    vector<vector<int> > combinationSum(vector<int> &candidates, int target) {
	        vector<vector<int>> result;
	        vector<int> path;
	        sort(candidates.begin(), candidates.end());
	        combinationSumHelper(result, path, 0, candidates, target);
	        return result;
	    }
	    void combinationSumHelper(vector<vector<int>> &result, vector<int> &path, int pos, vector<int> &candidates, int target){
	        if(accumulate(path.begin(),path.end(), 0)== target){
	            result.push_back(path);
	            return;
	        }
	        if(accumulate(path.begin(),path.end(), 0)> target){
	            return;
	        }
	        for(int i = pos; i < candidates.size(); i++){
	            path.push_back(candidates[i]);
	            combinationSumHelper(result, path, i, candidates, target);
	            path.pop_back();
	        }
	    }
	};

   