---
layout: post
title: "matrix-zigzag-traversal"
tags: [算法]
---	
	
>http://www.lintcode.com/en/problem/matrix-zigzag-traversal/

找边界条件：

向下和向上标识
上下左右四个边界转移方向

	class Solution {
	public:
	    /**
	     * @param matrix: a matrix of integers
	     * @return: a vector of integers
	     */
	    vector<int> printZMatrix(vector<vector<int> > &matrix) {
	        // write your code here
	        vector<int> result;
	        int m = matrix.size();
	        if (m == 0) return result;
	        int n = matrix[0].size();
	        if (n == 0) return result;
	        int i = 0;
	        int j = 0;
	        int upflag = 1;
	        while ((i + j) <= (m + n - 2)) {
	            result.push_back(matrix[i][j]);
	            if ((i == 0 || j == n-1) && upflag) {
	                if (j == n-1) i++;
	                else
	                    j++;
	                upflag = 0;
	            } else if ((j == 0 || i == m-1) && !upflag) {
	                if (i == m-1) j++;
	                else 
	                    i++;
	                upflag = 1;
	            } else if (upflag == 1){
	                i--;
	                j++;
	            } else {
	                i++;
	                j--;
	            }
	        }
	        return result;
	    }
	};



参考资料

+ http://www.jyuan92.com/blog/careercup1_1-unique-characters/