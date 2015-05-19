---
layout: post
title: "maximum-depth-of-binary-tree"
tags: [算法]
---	
	
>http://www.lintcode.com/en/problem/maximum-depth-of-binary-tree/

分治

	/**
	 * Definition of TreeNode:
	 * class TreeNode {
	 * public:
	 *     int val;
	 *     TreeNode *left, *right;
	 *     TreeNode(int val) {
	 *         this->val = val;
	 *         this->left = this->right = NULL;
	 *     }
	 * }
	 */
	 
	class Solution {
	public:
	    /**
	     * @param root: The root of binary tree.
	     * @return: An integer
	     */
	    int maxDepth(TreeNode *root) {
	        // write your code here
	    if(root == NULL) return 0;  
	        return 1 + max( maxDepth(root->left), maxDepth(root->right) ); 
	    }
	};


开始写的一个错误答案

	/**
	 * Definition of TreeNode:
	 * class TreeNode {
	 * public:
	 *     int val;
	 *     TreeNode *left, *right;
	 *     TreeNode(int val) {
	 *         this->val = val;
	 *         this->left = this->right = NULL;
	 *     }
	 * }
	 */
	void maxDepthHelper(TreeNode *root, int &result, int Depth) {
	    if (Depth > result) result = Depth;
	    if (!root) return;
	    if (!root->left) {
	        maxDepthHelper(root->left, result, Depth+1);
	    }
	    if (!root->right) {
	        maxDepthHelper(root->left, result, Depth+1);
	    }
	}
	class Solution {
	public:
	    /**
	     * @param root: The root of binary tree.
	     * @return: An integer
	     */
	    int maxDepth(TreeNode *root) {
	        // write your code here
	        int result = 0;
	        int Depth = 0;
	        maxDepthHelper(root, result, Depth);
	        return result;
	    }
	};


参考资料：

+ http://blog.csdn.net/ithomer/article/details/8799795
+ 复习深度优先和广度优先算法http://www.blogjava.net/fancydeepin/archive/2013/02/03/395073.html