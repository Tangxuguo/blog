---
layout: post
title: "insert-interval"
tags: [算法]
---	
	
>http://www.lintcode.com/en/problem/insert-interval/


以原来的区间做迭代，应该可以用二分查找

注意：情况划分，特别是最后重叠的部分，符合merge，如何剔除，以及最后的最后的添加


	/**
	 * Definition of Interval:
	 * classs Interval {
	 *     int start, end;
	 *     Interval(int start, int end) {
	 *         this->start = start;
	 *         this->end = end;
	 *     }
	 */
	class Solution {
	public:
	    /**
	     * Insert newInterval into intervals.
	     * @param intervals: Sorted interval list.
	     * @param newInterval: new interval.
	     * @return: A new interval list.
	     */
	    vector<Interval> insert(vector<Interval> &intervals, Interval newInterval) {
	        // write your code here
	        if (intervals.size() == 0) {
	            intervals.push_back(newInterval);
	            return intervals;
	        }
	        vector<Interval>::iterator it = intervals.begin();
	        while (it != intervals.end()) {
	            // front of current interval
	            if (newInterval.end < it->start) {
	                intervals.insert(it, newInterval);
	                return intervals;
	            } 
	            // behind of current interval
	            else if (newInterval.start > it->end) {
	                it++;
	                continue;
	            }
	            // overlap, current interval merge to newInterval, waiting next
	            else {
	                newInterval.start = min(newInterval.start, it->start);
	                newInterval.end = max(newInterval.end, it->end);
	                it =intervals.erase(it);
	            }
	        }
	        // end of intervals
	        intervals.insert(intervals.end(), newInterval);
	        return intervals;
	    }
	};
	


参考资料：

+ http://fisherlei.blogspot.com/2012/12/leetcode-insert-interval.html