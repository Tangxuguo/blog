---
layout: post
title: "median-of-two-sorted-arrays"
tags: [算法]
---	
	
>http://www.lintcode.com/en/problem/median-of-two-sorted-arrays/
	
		double findKth(vector<int> A, int A_start, vector <int> B, int B_start, int k) {
	    // A_start come to end ,so find kth in B
	    if (A_start >= A.size()) {
	        return B[B_start + k-1];
	    }
	    // B_start come to end ,so find kth in A
	    if (B_start >= B.size()) {
	        return A[A_start + k-1];
	    }
	    // find 1st between A and B, just min( A[A_start], B[B_start])
	    if (k == 1) {
	        return  A[A_start]< B[B_start]?A[A_start]: B[B_start];
	    }
	    // divide k into two parts
	    // note: k is also Even
	    int A_move = (A_start + k / 2 )< A.size() ?k/2:(A.size()-A_start);
	    int B_move = k - A_move;
	    
	    if (A[A_start + A_move - 1] < B[B_start + B_move - 1]) {
	        return findKth(A, A_start + A_move, B, B_start, k - A_move);
	    }
	    else if (A[A_start + A_move - 1] > B[B_start + B_move - 1]) {
	        return findKth(A, A_start, B, B_start + B_move, k - B_move);
	    }
	    else
	        return A[A_start + A_move - 1];
	}
	
	class Solution {
	public:
	    /**
	     * @param A: An integer array.
	     * @param B: An integer array.
	     * @return: a double whose format is *.5 or *.0
	     */
	    double findMedianSortedArrays(vector<int> A, vector<int> B) {
	        // write your code here
	        int total = A.size() + B.size();
	        // even
	        if (total & 0x1) {
	            return findKth(A, 0, B, 0, total / 2 + 1);
	        }
	        // odd
	        else {
	            return (findKth(A, 0, B, 0, total / 2) + findKth(A, 0, B, 0, total / 2 + 1)) / 2;
	        }
	        double aa=findKth(A, 0, B, 0, total / 2 + 1);
	  
	    
	    }
	};	
	
	
参考资料

+ http://blog.csdn.net/yutianzuijin/article/details/11499917

+ http://blog.csdn.net/zxzxy1988/article/details/8587244
+ http://www.jiuzhang.com/solutions/median-of-two-sorted-arrays/