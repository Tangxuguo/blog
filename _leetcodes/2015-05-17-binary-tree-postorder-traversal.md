---
layout: post
title: "binary-tree-postorder-traversal"
tags: [算法]
---	
	
>http://www.lintcode.com/en/problem/binary-tree-postorder-traversal/

非递归遍历树总结：

1. 前序遍历不需要增加标志
2. 中序遍历需要增加一个标志
3. 后序就需要增加两个标志
难度也是一个比一个难。




后序取巧办法

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
	    /**
	     * @param root: The root of binary tree.
	     * @return: Postorder in vector which contains node values.
	     */
	public:
	    vector<int> postorderTraversal(TreeNode *root) {
	        // write your code here
	        vector<int> result;
	        stack<TreeNode*> tmp;
	        if(root) {
	            tmp.push(root);
	        }
	        while(!tmp.empty()) {
	
	            TreeNode* pNode = tmp.top();
	            result.push_back(pNode->val);
	            tmp.pop();
	            
	            if(pNode->left) {
	                tmp.push(pNode->left);
	            }
	            if(pNode->right) {
	                tmp.push(pNode->right);
	            }
	        }
	        reverse(result.begin(), result.end());
	        return result;
	    }
	};



参考资料：

+ http://blog.csdn.net/kenden23/article/details/14526023