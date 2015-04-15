---
layout: post
title: "search-a-2d-matrix-ii"
tags: [算法]
---

>http://lintcode.com/en/problem/search-a-2d-matrix-ii/	

表面上看起来很难，其实知道思想很简单，注意按对角线搜索，这里从左下方往右上方搜索，另外是特殊值处理
	
	class Solution {
	public:
	    /**
	     * @param matrix: A list of lists of integers
	     * @param target: An integer you want to search in matrix
	     * @return: An integer indicate the total occurrence of target in the given matrix
	     */
	    int searchMatrix(vector<vector<int> > &matrix, int target) {
	        // write your code here
	        int m = matrix.size();
	        if (m == 0) {
	            return 0;
	        }
	        int n = matrix[0].size();
	        int i, j;
	        int counter = 0;
	        for (i = m-1; i >= 0; i--) {
	            for (j = 0; j <n; j++) {
	                if (i<0 || j>=n) {
	                    return counter;
	                }
	                if (matrix[i][j] == target) {
	                    counter++;
	                    i--;
	                }
	                else if (matrix[i][j] > target) {
	                    i--;
	                    j--;
	                }
	            }
	        }
	        return counter;
	    }
	};
	
