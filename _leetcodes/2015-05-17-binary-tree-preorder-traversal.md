---
layout: post
title: "binary-tree-preorder-traversal"
tags: [算法]
---	
	
>http://www.lintcode.com/en/problem/binary-tree-preorder-traversal/


注意递归退出条件


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
	void preorderTraversalHelper(TreeNode *root, vector<int> &result) {
	    if (!root) return;
	    result.push_back(root->val);
	    preorderTraversalHelper(root->left, result);
	    preorderTraversalHelper(root->right, result);
	}
	
	class Solution {
	public:
	    /**
	     * @param root: The root of binary tree.
	     * @return: Preorder in vector which contains node values.
	     */
	    vector<int> preorderTraversal(TreeNode *root) {
	        // write your code here
	        vector<int> result;
	        preorderTraversalHelper(root, result);
	        return result;
	    }
	};


开始写了个这个，死循环了，报了错误“Memory Limit Exceeded”

	void preorderTraversalHelper(TreeNode *root, vector<int> &result) {
	    if (root == nullptr) return;
	    while (root != nullptr) {
	        result.push_back(root->val);
	        preorderTraversalHelper(root->left, result);
	        preorderTraversalHelper(root->right, result);
	    }
	}
	
非递归版本：

注意入栈顺序

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
	     * @return: Preorder in vector which contains node values.
	     */
	    vector<int> preorderTraversal(TreeNode *root) {
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
	            if(pNode->right)
	                tmp.push(pNode->right);
	            if(pNode->left)
	                tmp.push(pNode->left);
	        }
	        return result;
	    }
	};


复习stack和queue操作

###1、stack
stack模板类的定义在<stack>头文件中。
stack模板类需要两个模板参数，一个是元素类型，一个容器类型，但只有元素类型是必要的，在不指定容器类型时，默认的容器类型为deque。
定义stack对象的示例代码如下：
stack<int> s1;
stack<string> s2;

stack的基本操作有：
入栈，如例：s.push(x);
出栈，如例：s.pop();注意，出栈操作只是删除栈顶元素，并不返回该元素。
访问栈顶，如例：s.top()
判断栈空，如例：s.empty()，当栈空时，返回true。
访问栈中的元素个数，如例：s.size()

###2、queue
queue模板类的定义在<queue>头文件中。
与stack模板类很相似，queue模板类也需要两个模板参数，一个是元素类型，一个容器类型，元素类型是必要的，容器类型是可选的，默认为deque类型。
定义queue对象的示例代码如下：

queue<int> q1;
queue<double> q2;

queue的基本操作有：
入队，如例：q.push(x); 将x接到队列的末端。
出队，如例：q.pop(); 弹出队列的第一个元素，注意，并不会返回被弹出元素的值。
访问队首元素，如例：q.front()，即最早被压入队列的元素。
访问队尾元素，如例：q.back()，即最后被压入队列的元素。
判断队列空，如例：q.empty()，当队列空时，返回true。
访问队列中的元素个数，如例：q.size()。


参考资料：

+ http://bookshadow.com/weblog/2014/12/30/leetcode-factorial-trailing-zeroes/