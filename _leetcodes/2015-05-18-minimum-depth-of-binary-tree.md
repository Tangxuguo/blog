---
layout: post
title: "minimum-depth-of-binary-tree"
tags: [算法]
---	
	
>http://www.lintcode.com/en/problem/minimum-depth-of-binary-tree/

和找最大深度类似，不过要注意条件，由于是叶子节点，直接用min找会找到非叶子节点
1，树为空，则为0。 2，根节点如果只存在左子树或者只存在右子树，则返回值应为左子树或者右子树的（最小深度+1）。 3，如果根节点的左子树和右子树都存在，则返回值为（左右子树的最小深度的较小值+1）。



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
	    int minDepth(TreeNode *root) {
	        // write your code here
	        if(root == NULL) return 0;
	        if(root -> left == NULL && root -> right == NULL) return 1;
	        if(root -> left != NULL && root -> right == NULL) return 1+minDepth(root->left);
	        if(root -> left == NULL && root -> right != NULL) return 1+minDepth(root->right);
	        int left = minDepth(root->left);
	        int right = minDepth(root->right);
	        return min(left+1,right+1);
	    }
	};

非递归方法：基于广度优先搜索用队列

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
	    int minDepth(TreeNode *root) {
	        if (root == NULL) return 0;
	        queue<TreeNode *> nodeQueue;
	        nodeQueue.push(root);
	        int level = 1;
	        int currNum = 1; //num of nodes left in current level  
	        int nextNum = 0; //num of nodes in next level 
	        while (!nodeQueue.empty()) {
	            TreeNode * pNode = nodeQueue.front();
	            nodeQueue.pop();
	            currNum--;
	            if(pNode->left == NULL && pNode->right == NULL) return level;  
	            if(pNode->left != NULL) {
	                nodeQueue.push(pNode->left);
	                nextNum++;
	            }
	            if(pNode->right != NULL) {
	                nodeQueue.push(pNode->right);
	                nextNum++;
	            }
	            if(currNum == 0) {
	                currNum = nextNum;
	                nextNum = 0;
	                level++;
	            }
	        }
	        return level;
	        
	    }
	};
