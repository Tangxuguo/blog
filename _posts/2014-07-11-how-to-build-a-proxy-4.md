---
layout: post
title: 搭建代理(4) 使用VPN（PPTP）
categories: [学习]
tags: [代理]
---


前面我们购买的VPS是支持pptp的，所以按照以下教程安装即可

这里转载博文[http://hengzhang.cn/0323153.html](http://hengzhang.cn/0323153.html)

一个自动安装PPTP VPN的安装脚本，适用于Debian或Ubuntu系统，支持OpenVZ架构的VPS。
#1，检查PPP模块
root帐户，通过SSH登录到你的OpenVZ VPS，敲入：

	cat /dev/ppp

如返回下面一行，则表示PPP模块是开启着的。

	cat: /dev/ppp: No such device or address

#2，下载并运行脚本：


	wget http://dadi.me/wp-content/uploads/dir/VPN/pptp_ovz_debian.sh
	chmod +x pptp_ovz_debian.sh
	./pptp_ovz_debian.sh

	***安装选项简介***
	
	
	1) Set up new PoPToP server AND create one user
	# 键入1，回车、则安装PPTP服务，并创建一个用户。
	2) Create additional users
	# 键入2，回车、则添加用户。
	Enter username that you want to create (eg. vpn or dadi.me):
	# 需要创建的VPN登录所需的用户名。
	Specify password that you want the server to use:
	# 对应VPN用户的密码。

#3，最后输入密码之后就开始自动安装了，或是添加用户。
