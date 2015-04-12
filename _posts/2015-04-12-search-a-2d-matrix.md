---
layout: post
title: "search-a-2d-matrix"
tags: [算法]
---

>http://www.lintcode.com/en/problem/search-a-2d-matrix/	
	
	class Solution {
	public:
	    /**
	     * @param matrix, a list of lists of integers
	     * @param target, an integer
	     * @return a boolean, indicate whether matrix contains target
	     */
	    bool searchMatrix(vector<vector<int> > &matrix, int target) {
	        // write your code here
	        int start = 0;
	        int end = matrix.size()-1;
	        int mid;
	        int row = 0;
	        if (matrix.size() == 0) {
	            return false;
	        }
	        if (matrix.size() > 1) {
	            while (start+1 < end) {
	                mid = start + (end - start)/2;
	                if (matrix[mid][0] == target) {
	                    return true;
	                }
	                else if (matrix[mid][0] > target) {
	                    end = mid;
	                }
	                else {
	                    start = mid;
	                }
	            }
	            if (matrix[start][0] == target) {
	                return true; 
	            }
	            else if (matrix[end][0] == target) {
	                return true;
	            }
	            else if (matrix[start][0] > target) {
	                row = start -1;
	            }
	            else if (matrix[end][0] < target) {
	                row = end;
	            }
	            else {
	                row = start;
	            }
	        }
	        start = 0;
	        end = matrix[row].size()-1;
	        while (start + 1 < end) {
	            mid = start + (end - start)/2;
	            if (matrix[row][mid] == target) {
	                return true;
	            }
	            else if (matrix[row][mid] > target) {
	                end = mid;
	            }
	            else {
	                start = mid;
	            }
	        }
	        if (matrix[row][start] == target) {
	            return true;
	        }
	        else if (matrix[row][end] == target) {
	            return true;
	        }
	        else {
	            return false;
	        }
	    }
	};
	
