---
layout: post
title: "gas-station"
tags: [算法]
---	
	
>http://www.lintcode.com/en/problem/gas-station/

做这道题的关键是要可以总结出来这道题目的属性，注意Note这个地方，其属性主要有两个：
1 如果总的gas - cost小于零的话，那么没有解返回-1
2 如果前面所有的gas - cost加起来小于零，那么前面所有的点都不能作为出发点。

O(n^2)时间复杂度,编译出错

	class Solution {
	public:
	    /**
	     * @param gas: a vector of integers
	     * @param cost: a vector of integers
	     * @return: an integer 
	     */
	    int canCompleteCircuit(vector<int> &gas, vector<int> &cost) {
	        // write your code here
	        int i, j;
	        for (i = 0; i < cost.size(); i++) {
	            int car = 0;  // current gas
	            for (j = i; j < i + cost.size(); j++) {
	                car = car + gas[j % cost.size()];
	                if (car < cost[j % cost.size()]) {
	                     break;
	                }
	                car = car - cost[j % cost.size()];
	            }
	            // availiable i
	            if (j == i + cost.size()) {
	                return i;
	            }
	        }
	        return -1;
	    }
	};

解法二，跳跃式，自己控制跳跃，不用自增

	class Solution {
	public:
	    /**
	     * @param gas: a vector of integers
	     * @param cost: a vector of integers
	     * @return: an integer 
	     */
	    int canCompleteCircuit(vector<int> &gas, vector<int> &cost) {
	        // write your code here
	        for (int i = 0; i < cost.size(); )  
	        {  
	            int leftGas = 0;  
	            int j = 0;  
	            for (; j < cost.size(); j++)  
	            {  
	                int k = (i+j)%cost.size();  
	                leftGas += (gas[k] - cost[k]);  
	                if (leftGas < 0) break;  
	            }  
	            if ( j == cost.size()) return i;  
	            i+=j+1;  
	        }  
	        return -1;  
	    }
	};

第二种写法：

为什么返回的时候加1，因为是从下一个开始的，那会不会出现i+1不存在呢？不会，因为如果下一个不存在的话，那说明前面的都不满足也就是，这个leftGas<0,

	class Solution {
	public:
	    /**
	     * @param gas: a vector of integers
	     * @param cost: a vector of integers
	     * @return: an integer 
	     */
	    int canCompleteCircuit(vector<int> &gas, vector<int> &cost) {
	        // write your code here
	        int leftGas = 0;
	        int sum = 0;
	        int diff = 0;
	        int start = 0;
	        
	        for (int i = 0; i < cost.size(); i++)  
	        {  
	            diff = (gas[i] - cost[i]);
	            sum += diff;
	            leftGas += diff;
	            if (sum < 0) {
	                start = i+1;
	                sum = 0;
	            }  
	        }
	        if (leftGas < 0) {
	            return -1;
	        }
	        else {
	            return start;
	        }
	    }
	};


参考资料：

+ http://fisherlei.blogspot.com/2013/11/leetcode-gas-station-solution.html
+ http://blog.csdn.net/kenden23/article/details/14106137