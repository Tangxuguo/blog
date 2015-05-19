---
layout: post
title: "insert-node-in-a-binary-search-tree"
tags: [算法]
---	
	
>http://www.lintcode.com/en/problem/insert-node-in-a-binary-search-tree/

通过

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
	     * @param root: The root of the binary search tree.
	     * @param node: insert this node into the binary search tree
	     * @return: The root of the new binary search tree.
	     */
	    TreeNode* insertNode(TreeNode* root, TreeNode* node) {
	        // write your code here
	        TreeNode *prev = root;
	        TreeNode *t = root;
	        if (!root) {
	            return node;
	        }
	        while (NULL != t) {
	            prev = t;
	            if (t -> val > node -> val) {
	                t = t -> left;
	            }
	            else {
	                t = t -> right;
	            }
	        }
	        if (NULL != prev) {
	            if (prev -> val > node -> val) {
	                prev -> left = node;
	            }
	            else {
	                prev -> right = node;
	            }
	        }
	        return root;
	    }
	};

未通过，这说明 ！不是乱用

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
	     * @param root: The root of the binary search tree.
	     * @param node: insert this node into the binary search tree
	     * @return: The root of the new binary search tree.
	     */
	    TreeNode* insertNode(TreeNode* root, TreeNode* node) {
	        // write your code here
	        TreeNode *prev = root;
	        TreeNode *t = root;
	        if (!root) {
	            return node;
	        }
	        while (!t) {
	            prev = t;
	            if (t -> val > node -> val) {
	                t = t -> left;
	            }
	            else {
	                t = t -> right;
	            }
	        }
	        if (!prev) {
	            if (prev -> val > node -> val) {
	                prev -> left = node;
	            }
	            else {
	                prev -> right = node;
	            }
	        }
	        return root;
	    }
	};

通过

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
	     * @param root: The root of the binary search tree.
	     * @param node: insert this node into the binary search tree
	     * @return: The root of the new binary search tree.
	     */
	    TreeNode* insertNode(TreeNode* root, TreeNode* node) {
	        // write your code here
	        TreeNode *t = root;
	        if (!root) {
	            return node;
	        }
	        TreeNode* tempNode = root;
	        while (NULL != tempNode) {
	            if (node->val <= tempNode->val) {
	                if (NULL == tempNode->left) {
	                    tempNode->left = node;
	                    return root;
	                }
	                tempNode = tempNode->left;
	            } else {
	                if (NULL == tempNode->right) {
	                    tempNode->right = node;
	                    return root;
	                }
	                tempNode = tempNode->right;
	            }
	        }
	
	        return root;
	    }
	};
