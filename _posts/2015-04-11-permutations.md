---
layout: post
title: "permutations"
tags: [算法]
---
	

>http://lintcode.com/en/problem/permutations/

DFS & 回朔法，排列问题

注意：排列问题要记住所有已经访问过的点，当所有点都被访问过后可以退出

	
	class Solution {
	public:
	    /**
	     * @param nums: A list of integers.
	     * @return: A list of permutations.
	     */
	    vector<vector<int> > permute(vector<int> nums) {
	        // write your code here
	        vector<vector<int> > result;
	        vector<int > path;
	        if(nums.size()==0) {
	            return result;
	        }
	        vector<int> visited(nums.size(), 0); 
	        permuteHelper(result, path, visited, 0, nums);
	        return result;
	    }
	    void permuteHelper(vector<vector<int >> &result, vector<int> &path, vector<int> &visited, int pos,vector<int> &S){
	        if ( path.size() == S.size() ) {
	            result.push_back(path);
	        }
	        for (int i = 0; i< S.size(); i++){
	            if(visited[i]==0){
	                visited[i]=1;
	                path.push_back(S[i]);
	                permuteHelper(result, path, visited, i+1 , S);
	                path.pop_back();
	                visited[i]=0; 
	            }
	        }
	    }
	};

