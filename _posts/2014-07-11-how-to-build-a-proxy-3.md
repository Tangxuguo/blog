---
layout: post
title: 搭建代理(3) 使用cow局域网共享http代理
categories: [学习]
tags: [代理]
---


#cow介绍
cow的[github主页]( https://github.com/cyfdecyf/cow)

使用cow的原因，是因为我们不想每次都打开shadowsocks客户端，我们希望能够使用http代理，另外针对国内的网站我们不想使用代理，这样我们就可以在局域网内搭建一个服务器作为中转，需要说明的是千万不要在国外服务器实现socks5转http，容易被墙


COW 是一个简化穿墙的 HTTP 代理服务器。它能自动检测被墙网站，仅对这些网站使用二级代理
功能COW 的设计目标是自动化，理想情况下用户无需关心哪些网站无法访问，可直连网站也不会因为使用二级代理而降低访问速度。

*	 作为 HTTP 代理，可提供给移动设备使用；若部署在国内服务器上，可作为 APN 代理
* 支持 HTTP, SOCKS5, shadowsocks 和 cow 自身作为二级代理

		* 可使用多个二级代理，支持简单的负载均衡

* 自动检测网站是否被墙，仅对被墙网站使用二级代理
* 自动生成包含直连网站的 PAC，访问这些网站时可绕过 COW
* 
		* 内置常见可直连网站，如国内社交、视频、银行、电商等网站（可手工添加）

<img src="/blog/public/images/posts/how-to-build-a-proxy/Image12.png" >

#安装

##新建局域网服务器
这里我在局域网里新建了一个虚拟机，并且安装了debian7

##安装shadowsocks
首先，你需要安装shadowsocks客户端
Debian / Ubuntu:

	apt-get install python-pip python-m2crypto
	pip install shadowsocks

编辑配置文件 vim /etc/shadowsocks.json

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


后台运行`sslocal -c /etc/shadowsocks.json &`

	注：如果你不需要socks5转http，也不需要针对国内网站采取直连的方式
	那么你可以把`"local_address": "127.0.0.1"`改成`"local_address": "0.0.0.0"`,这样服务器就可以侦听1080端口，也不用进行后面的操作
	客户端设置成<img src="/blog/public/images/posts/how-to-build-a-proxy/Image12.jpg" >

##安装cow
然后安装cow，

OS X, Linux (x86, ARM): 执行以下命令（也可用于更新）

`curl -L git.io/cow | bash`


编辑 ~/.cow/rc (Linux) 或 rc.txt (Windows)，简单的配置例子如下：

		#开头的行是注释，会被忽略
		# 本地 HTTP 代理地址
		# 配置 HTTP 和 HTTPS 代理时请填入该地址
		# 或者在自动代理配置中填入 http://127.0.0.1:7777/pac#监听本地请求
		#listen = http://127.0.0.1:7777listen =
		#监听局域网请求
		listen = http://0.0.0.0:7777
		
		# SOCKS5 二级代理
		proxy = socks5://127.0.0.1:1080
		# HTTP 二级代理
		#proxy = http://127.0.0.1:8080
		#proxy = http://user:password@127.0.0.1:8080
		# shadowsocks 二级代理
		#proxy = ss://aes-128-cfb:password@1.2.3.4:8388
		# cow 二级代理
	    #proxy = cow://aes-128-cfb:password@1.2.3.4:8388

这里我们只配置了监听端口`listen = http://0.0.0.0:7777`以及shadowsocks的socks5代理`proxy = socks5://127.0.0.1:1080`

完成配置后启动 COW 并配置好代理即可使用。

linux的启动脚本：下载脚本保存为cow https://github.com/cyfdecyf/cow/blob/master/doc/init.d/cow
在命令行上执行 cow &

这样这台机子一直在监听7777号端口，其他客户端同样使用switchsharp添加http代理，这样就可以了
<img src="/blog/public/images/posts/how-to-build-a-proxy/Image13.png" >


