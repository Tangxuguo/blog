---
layout: post
title: "majority-number"
tags: [算法]
---	
	
>http://www.lintcode.com/en/problem/majority-number/

最简单的直接排序后，取中位数

时间复杂度O(nlogn),空间O(1)

	class Solution {
	public:
	    /**
	     * @param nums: A list of integers
	     * @return: The majority number
	     */
	    int majorityNumber(vector<int> nums) {
	        // write your code here
	        sort(nums.begin(), nums.end());
	        return nums[nums.size()/2];
	    }
	};

用HASH MAP,时间复杂度O(n),空间O(m)

Moore’s Voting Algorithm：

>stack overflow的提示。 试想， 既然是majority element， 那么用这个majority element同那些非majority 相对应， 遇到不是majority的element， 就用这个majority element将其抵消掉（假如我们知道那一个数是majority）， 最后剩下的数字必然只有我们的majority element。 当然算法运行前， 我们并不知道哪一个数是majority element。 所以我们就把相异的元素抵消掉， 最后剩下的元素就是我们的majority element了。


	class Solution {
	public:
	    /**
	     * @param nums: A list of integers
	     * @return: The majority number
	     */
	    int majorityNumber(vector<int> nums) {
	        // write your code here
	        int counter = 1;
	        int curr = 0;
	        for (int i = 0; i < nums.size(); i++) {
	            (nums[curr] == nums[i]) ? ++counter:--counter;
	            if(counter == 0) {
	                curr = i;
	                counter = 1;
	            }
	        }
	        return nums[curr];
	    }
	};

如果不放心，可以对上面的majority在遍历一遍数组统计

另外如果是刚好一半的话，有一个结论

>易知总数必定是偶数，同时删除不同数字，最后剩余的两个数字必有其一为水王，只需简单判断一下即可

参考资料：

http://blog.csdn.net/a130737/article/details/39781951
http://taop.marchtea.com/21.0.html
http://www.cs.utexas.edu/~moore/best-ideas/mjrty/index.html