---
layout: post
title: 手把手叫你搭建代理(2) 使用shadowsocks提供外网代理
categories: [学习]
tags: [学习]
---

#安装debian操作系统
前面我们使用paypal购买了vps，默认给的是centos，接下来在控制面板里选择新建系统，在这里我们选择debian7 32位，为什么选择debian，因为占用资源小，稳定，7是目前最新的，32位因为软件兼容性好，重装一会就好了
<img src="/blog/public/images/posts/how-to-build-a-proxy/Image8.png" >
装好后，会给你用户名和密码，以及ip地址ssh端口号，通过ssh客户端登录系统，这里window下推荐putty
<img src="/blog/public/images/posts/how-to-build-a-proxy/Image9.png" >
#安装shadowsocks
准备工作做好后，开始安装shadowsocks
去shadowsocks的[github主页](https://github.com/clowwindy/shadowsocks)
了解shadowsocks请自行谷歌（官网http://shadowsocks.org/）
shadowsocks分为客户端和服务端，客户端和服务端又有很多不同的版本，用的比较多的是python和libev版本

##python版：
####server端：

使用我们刚才建立的debian系统

#####手动启动版：
执行

 	
		 apt-get update
		 apt-get install python-pip python-m2crypto 
		 pip install shadowsocks
 

编辑 /etc/shadowsocks.json

		{
		    "server":"my ip",
		    "server_port":8388,
		    "local_port":1080,
		    "password":"my password",
		    "timeout":600,
		    "method":"aes-256-cfb"
		}

`ssserver -c /etc/shadowsocks.json &`

#####开机自启动版：

参考shadowsocks[github主页](https://github.com/clowwindy/shadowsocks/wiki/%E7%94%A8-Supervisor-%E8%BF%90%E8%A1%8C-Shadowsocks0)

如果你想在后台运行 Shadowsocks，可以使用 Supervisor，它可以方便地监控和控制任何程序，实现开机自动启动和后台运行。
下面是 Debian 和 Ubuntu 上的配置方法（如果你是 Linux 新手，不需要 SELinux，也不了解防火墙配置，也弄不清楚 RHEL 和 CentOS 的版本机制，那就用 Debian 或 Ubuntu 吧，别折腾了。 ）：
执行

		apt-get update
		apt-get install python-pip python-m2crypto supervisor
		pip install shadowsocks

编辑 /etc/shadowsocks.json

	{
	    "server":"my ip",
	    "server_port":8388,
	    "local_port":1080,
	    "password":"my password",
	    "timeout":600,
	    "method":"aes-256-cfb"
	}

编辑 /etc/supervisor/conf.d/shadowsocks.conf

	[program:shadowsocks]
	command=ssserver -c /etc/shadowsocks.json
	autorestart=true
	user=nobody

如果端口 < 1024，把上面的 user=nobody 改成 user=root。
在 /etc/default/supervisor 最后加一行：

`ulimit -n 51200`

执行

	service supervisor start
	supervisorctl reload


这样服务器端就弄好了。

####客户端：
#####window客户端:
推荐[shadowsocks-gui](https://github.com/shadowsocks/shadowsocks-gui)

去[sourceforge](http://sourceforge.net/projects/shadowsocksgui/files/dist/)下载自己系统对应的编译好的版本
我选择的是shadowsocks-gui-0.4.1-win-ia32.7z

解压后运行shadowsocks.exe，设置好参数
<img src="/blog/public/images/posts/how-to-build-a-proxy/Image10.png" >



#####linux客户端：
建议使用python版本不折腾https://pypi.python.org/pypi/shadowsocks

Debian / Ubuntu:

	apt-get install python-pip python-m2crypto
	pip install shadowsocks

编辑配置文件 `vim /etc/shadowsocks.json`

	{
	    "server":"my_server_ip",
	    "server_port":8388,
	    "local_address": "127.0.0.1",
	    "local_port":1080,
	    "password":"mypassword",
	    "timeout":300,
	    "method":"aes-256-cfb",
	    "fast_open": false,
	    "workers": 1
	}


运行`sslocal -c /etc/shadowsocks.json`

客户端弄好之后就可以使用了，这里推荐使用chrome的switchsharp，当然只要是webkit内核，基本都支持这个插件
然后在switchsharp里新建情景模式，配置如下，注意只能选socks5代理，其他为空
然后打开youtube.com,测试下是否好了
<img src="/blog/public/images/posts/how-to-build-a-proxy/Image11.png" >
##libev版本
使用libev版本，如果你爱折腾的话，可以从官方下载源码后编译，如果想省事，可以直接下载官方的编译版本

使用官方版本：

编辑文件 /etc/apt/sources.list

添加源

	# Debian Wheezy, Ubuntu 12.04 or any distribution with libssl > 1.0.1
	deb http://shadowsocks.org/debian wheezy main
	
	# Debian Squeeze, Ubuntu 11.04, or any distribution with libssl > 0.9.8, but < 1.0.0
	deb http://shadowsocks.org/debian squeeze main

安装shadowsocks

	sudo apt-get update
	sudo apt-get install shadowsocks
	
	# Edit the configuration
	sudo vim /etc/shadowsocks/config.json
	
	# Start the service
	sudo /etc/init.d/shadowsocks start