---
layout: post
title: "restore-ip-addresses"
tags: [算法]
---
	

>https://leetcode.com/problems/restore-ip-addresses/

DFS & 回朔法，划分问题

注意：剔除 0x和00x型IP，回朔注意删除刚才添加的数据,递归注意退出条件

	
	class Solution {
	public:
	    vector<string> restoreIpAddresses(string s) {
	        vector<string> result;
	        if (s.length()<4){
	            return result;
	        }
	        string path;
	        restoreIpAddressesHelper(result , 0, 0, path, s);
	        return result;
	    }
	    void restoreIpAddressesHelper(vector<string> &result,int pos, int step, string &path, string &s){
	        if ( pos == s.length() && step==4) {
	        result.push_back(path.substr(0,path.length()-1));
	            return;
	        }
	        
	        if (step<4){
	            for (int j = 1; j<=3; j++ ) {            
	                if ((pos+j)<=s.length() && (stoi(s.substr(pos, j) )<256)){
	                    string ele=s.substr(pos, j);
	                    if(ele.length()>=2&&ele[0]=='0'){
	                        continue;
	                    }
	                    path+= s.substr(pos, j);
	                    path+=".";
	                    restoreIpAddressesHelper(result, pos+j, step+1, path, s);
	                    path=path.substr(0, path.length()-j-1);
	                }
	                if((pos+j)>s.length()) {
	                    break;
	                }
	            }  
	        }
	    }
	};