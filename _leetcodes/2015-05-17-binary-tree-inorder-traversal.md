---
layout: post
title: "binary-tree-inorder-traversal"
tags: [算法]
---	
	
>http://www.lintcode.com/en/problem/binary-tree-inorder-traversal/

仔细阅读参考资料：

掌握两个栈的访问，以及标志位访问的方法

非递归遍历树总结：

	1. 前序遍历不需要增加标志
	2. 中序遍历需要增加一个标志
	3. 后序就需要增加两个标志
	难度也是一个比一个难。
	
	
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
	     * @return: Inorder in vector which contains node values.
	     */
	public:
	    vector<int> inorderTraversal(TreeNode *root) {
	        // write your code here
	        vector<int> rs;
	        if (!root) return rs;
	        stack<TreeNode *> stk;
	        TreeNode *p = root;
	        while (!stk.empty() || p) {
	            if (p) {
	                stk.push(p);
	                p = p->left;
	            } else {
	                p = stk.top();
	                stk.pop();
	                rs.push_back(p->val);
	                p = p->right;
	            }
	        }
	        return rs;
	    }
	};

加速，先递归到左子树最后一个，访问该节点，弹出，然后访问该节点右孩子，进入到下一次循环，如果有，访问右孩子的左子树，如果这个右孩子有左子树，访问，如果没有，把该右孩子的左孩子压栈访问，没有的话，只把右孩子压栈访问，如果连这个右孩子也没有，弹出倒数第二个树根，然后

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
	     * @return: Inorder in vector which contains node values.
	     */
	public:
	    vector<int> inorderTraversal(TreeNode *root) {
	        // write your code here
	        vector<int> rs;
	        if (!root) return rs;
	        stack<TreeNode *> stk;
	        TreeNode *p = root;
	        while (!stk.empty() || p) {
	            while (p) {
	                stk.push(p);
	                p = p->left;
	            }
	            p = stk.top();
	            stk.pop();
	            rs.push_back(p->val);
	            p = p->right;
	            
	        }
	        return rs;
	    }
	};

	



参考资料：

+ http://blog.csdn.net/kenden23/article/details/14161669