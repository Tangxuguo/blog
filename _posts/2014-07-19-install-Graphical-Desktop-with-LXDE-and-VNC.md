---
layout: post
title: "使用LXDE和VNC在小内存VPS安装图形化桌面"
tags: [linux]
---

LXDE 是一个非常轻量级的图形化桌面环境， 非常适合在小内存VPS上运行，比XFCE运行内存还小，据说运行内存最多不超过40M,了解更多信息访问[官网](http://www.lxde.org/)，我们选择它就是因为他内存小

这篇教程是关于如何在LINUX VPS server上安装图像化界面，以及让你使用VNC远程桌面控制

#1、服务器端安装

	# Make sure Debian is the latest and greatest
	
	apt-get update
	apt-get upgrade
	apt-get dist-upgrade
	
	# Install X, LXDE, VNC programs(500M)
	
	apt-get install xorg lxde-core tightvncserver
	
	# Start VNC to create config file
	
	tightvncserver :1
	
	# Then stop VNC
	
	tightvncserver -kill :1
	
	# Edit config file to start session with LXDE:
	
	nano ~/.vnc/xstartup
	
	# Add this at the bottom of the file:
	lxterminal &
	/usr/bin/lxsession -s LXDE &
	
	# Restart VNC
	
	tightvncserver :1

#2、客户端访问
window下使用 RealVNC，或者[TightVNC](http://www.tightvnc.com/download.html)

	[server ip]:5901 or [server ip]:1

点击连接，输入VNC密码

#附加
安装额外的包（firefox）
默认环境中的浏览器使用不喜欢，我们还是喜欢使用FIREFOX火狐浏览器，但是在DEBIAN系统中火狐浏览器叫做iceweasel，我们需要安装。(60M)

	apt-get install iceweasel
如果我们需要登录中文站点，还需要解决中文不乱码问题，就需要支持简体中文语言。(68M)

	apt-get install ttf-arphic-ukai ttf-arphic-uming ttf-arphic-gbsn00lp ttf-arphic-bkai00mp ttf-arphic-bsmi00lp
	
修改VNC密码

	vncpasswd
#开机启动tightvncserver
编辑tightvncserver
	
	nano /etc/init.d/tightvncserver
复制以下代码粘贴，user里填你想要运行tightvncserver的帐户
	
	#!/bin/sh
	### BEGIN INIT INFO
	# Provides: tightvncserver
	# Required-Start: $local_fs
	# Required-Stop: $local_fs
	# Default-Start: 2 3 4 5
	# Default-Stop: 0 1 6
	# Short-Description: Start/stop tightvncserver
	### END INIT INFO
	
	# More details see:
	# http://www.penguintutor.com/linux/tightvnc
	
	### Customize this entry
	# Set the USER variable to the name of the user to start tightvncserver under
	export USER=’root‘
	### End customization required
	
	eval cd ~$USER
	
	case “$1″ in
	start)
	su $USER -c ‘/usr/bin/tightvncserver :1′
	echo “Starting TightVNC server for $USER “
	;;
	stop)
	pkill Xtightvnc
	echo “Tightvncserver stopped”
	;;
	*)
	echo “Usage: /etc/init.d/tightvncserver {start|stop}”
	exit 1
	;;
	esac
	exit 0
然后运行

	sudo chmod 775 /etc/init.d/tightvncserver
	update-rc.d tightvncserver defaults
现在可以开机自动运行tighvncserver

#使用SSH转发
注意 (make sure tunneling is allowed in ssh config) 

	ssh -l user -p [ssh port usually 22] -L5901:localhost:5901 serverIp
Example (in client terminal) : 

	ssh -l root -p 22 -L5901:localhost:5901 10.10.0.1
goto vnc client and put localhost:5901