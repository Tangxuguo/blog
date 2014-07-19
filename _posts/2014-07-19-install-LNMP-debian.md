---
layout: post
title: "在debian7上安装lnmp环境"
tags: [linux]
---

#导入Dotdeb源
因为系统使用的官方源所安装的软件版本较低，所以这里推荐使用Dotdeb源。
在向源中导入Dotdeb源之前，我们需要先获取并导入GnuPG key：
	
	wget http://www.dotdeb.org/dotdeb.gpg
	cat dotdeb.gpg | apt-key add -

提示OK后则导入成功。然后我们开始导入Dotdeb源，使用vim打开 /etc/apt/sources.list：
	
	vim /etc/apt/sources.list
向其中添加如下两行：

	deb http://packages.dotdeb.org wheezy all
	deb-src http://packages.dotdeb.org wheezy all

添加之后，更新一下源：

	apt-get update
接下来就开始安装 MySQL + PHP + Nginx 了。
#1，安装MySQL
执行命令：

	apt-get install -y mysql-server mysql-client
即可安装MySQL，安装过程中会询问 root密码 ，键入你需要的密码之后回车即可。
安装完成后，执行如下命令进行一步安全设置：
	
	mysql_secure_installation
按照提示进行，过程中会询问是否更改 root密码，是否移除匿名用户，是否禁止root远程登录等。
#2，安装PHP
执行命令：

	apt-get install php5-fpm php5-gd php5-mysql php5-memcache php5-curl
上面的命令安装了php5-memcache的扩展，于是继续安装 Memcached 。

	apt-get install memcached
安装完毕之后，使用 php5-fpm -v 查看一下PHP的版本：

	~# php5-fpm -v
	PHP 5.4.16-1~dotdeb.1 (fpm-fcgi) (built: Jun  8 2013 22:20:42)
	Copyright (c) 1997-2013 The PHP Group
	Zend Engine v2.4.0, Copyright (c) 1998-2013 Zend Technologies

#3，安装Nginx

	apt-get install -y nginx-full
然后启动Nginx:

	service nginx start

访问结果如上图，接下来配置Nginx。

	vim /etc/nginx/sites-available/default
	……
	    location ~ \.php$ {
	        fastcgi_split_path_info ^(.+\.php)(/.+)$;
	    #    # NOTE: You should have "cgi.fix_pathinfo = 0;" in php.ini
	    #
	    #    # With php5-cgi alone:
	    #   fastcgi_pass 127.0.0.1:9000;
	    #    # With php5-fpm:
	        fastcgi_pass unix:/var/run/php5-fpm.sock;
	        fastcgi_index index.php;
	        include fastcgi_params;
	    }
	……

修改保存之后重启Nginx：

	service nginx restart
接下来我们新建一个phpinfo，查看php的详细信息：

	vim /usr/share/nginx/html/phpinfo.php
	<?php phpinfo(); ?>
保存之后访问

	 http://ip/phpinfo.php
如果出现 phpinfo 页面，则大功告成。
如何新建站点和军哥的一键包不同，此方法所安装的 LNMP 需要手动添加站点配置文件。

	cd /etc/nginx/conf.d 
进入配置文件目录，新建一个站点配置文件，比如 

	#vi dearroy.com.conf	

	server {
	    listen 80;
	        
	        #ipv6
	    #listen [::]:80 default_server;
	        
	    root /usr/share/nginx/html/dearroy.com;
	        
	        #默认首页文件名
	    index index.php index.html index.htm;
	    
	        #绑定域名
	    server_name localhost;
	        
	        #伪静态规则
	        include wordpress.conf;
	        
	    location / {
	        try_files $uri $uri/ /index.html;        
	    }
	        #定义错误页面
	    #error_page 404 /404.html;  
	        
	    location ~ \.php$ {
	        fastcgi_split_path_info ^(.+\.php)(/.+)$;
	        fastcgi_pass 127.0.0.1:9000;
	         fastcgi_index index.php;
	         include fastcgi_params;
	     }
	     #PHP
	}

保存之后重启Nginx，添加及绑定网站即完成。
最后，附两个最常用的程序Nginx伪静态：

	WordPress:
	location / {
	if (-f $request_filename/index.html){
	                rewrite (.*) $1/index.html break;
	        }
	if (-f $request_filename/index.php){
	                rewrite (.*) $1/index.php;
	        }
	if (!-f $request_filename){
	                rewrite (.*) /index.php;
	        }
	}

	Discuz X:
	rewrite ^([^\.]*)/topic-(.+)\.html$ $1/portal.php?mod=topic&topic=$2 last;
	rewrite ^([^\.]*)/article-([0-9]+)-([0-9]+)\.html$ $1/portal.php?mod=view&aid=$2&page=$3 last;
	rewrite ^([^\.]*)/forum-(\w+)-([0-9]+)\.html$ $1/forum.php?mod=forumdisplay&fid=$2&page=$3 last;
	rewrite ^([^\.]*)/thread-([0-9]+)-([0-9]+)-([0-9]+)\.html$ $1/forum.php?mod=viewthread&tid=$2&extra=page%3D$4&page=$3 last;
	rewrite ^([^\.]*)/group-([0-9]+)-([0-9]+)\.html$ $1/forum.php?mod=group&fid=$2&page=$3 last;
	rewrite ^([^\.]*)/space-(username|uid)-(.+)\.html$ $1/home.php?mod=space&$2=$3 last;
	rewrite ^([^\.]*)/([a-z]+)-(.+)\.html$ $1/$2.php?rewrite=$3 last;
	if (!-e $request_filename) {
	        return 404;


#优化Mysql

安装完mysql,默认占用内存达400多M, 服务器吃不消，调整参数优化到40-60M
	
	
	解决方案:调整以下参数 
	----------------
	performance_schema_max_table_instances=600
	table_definition_cache=400
	table_open_cache=256
	
	
	这样下来,mysql5.6.12就只使用  40---60M左右的内存了.
	
	
	以下是5.6默认的设置
	performance_schema_max_table_instances 12500
	table_definition_cache 1400
	table_open_cache 2000
#Nginx多站点配置
很多时候我们的服务器上不止放一个站点，如何进行多站点配置，这里列举两个
##增加A记录
去域名服务商那里，设置A记录指向服务器IP
##配置Nginx
###1.
nginx.conf内容如下:
	
	worker_processes 1;
	
	error_log /host/nginx/logs/error.log crit;
	
	pid /host/nginx/logs/nginx.pid;
	
	events {
	worker_connections 64;
	}
	
	http {
	include /host/nginx/conf/mime.types;
	default_type application/octet-stream;
	
	#charset gb2312;
	
	server_names_hash_bucket_size 128;
	client_header_buffer_size 32k;
	large_client_header_buffers 4 32k;
	
	keepalive_timeout 60;
	
	fastcgi_connect_timeout 300;
	fastcgi_send_timeout 300;
	fastcgi_read_timeout 300;
	fastcgi_buffer_size 128k;
	fastcgi_buffers 4 128k;
	fastcgi_busy_buffers_size 128k;
	fastcgi_temp_file_write_size 128k;
	client_body_temp_path /host/nginx/client_body_temp;
	proxy_temp_path /host/nginx/proxy_temp;
	fastcgi_temp_path /host/nginx/fastcgi_temp;
	
	gzip on;
	gzip_min_length 1k;
	gzip_buffers 4 16k;
	gzip_http_version 1.0;
	gzip_comp_level 2;
	gzip_types text/plain application/x-javascript text/css application/xml;
	gzip_vary on;
	
	client_header_timeout 3m;
	client_body_timeout 3m;
	send_timeout 3m;
	sendfile on;
	tcp_nopush on;
	tcp_nodelay on;
	#设定虚拟主机
	include /host/nginx/conf/vhost/www_test_com.conf;
	include /host/nginx/conf/vhost/www_test1_com.conf;
	include /host/nginx/conf/vhost/www_test2_com.conf;
	}
	
	
	
###2.
在conf目录下建立个vhost目录,在vhost目录下分别建立www_test_com.conf,www_test1_com.conf,www_test2_com.conf 3个文件
	
www_test_com.conf代码如下:
	
	server {
	listen 202.***.***.***:80; #换成你的IP地址
	client_max_body_size 100M;
	server_name www.test.com; #换成你的域名
	charset gb2312;
	index index.html index.htm index.php;
	root /host/wwwroot/test; #你的站点路径
	
	#打开目录浏览，这样当没有找到index文件，就也已浏览目录中的文件
	autoindex on;
	
	if (-d $request_filename) {
	rewrite ^/(.*)([^/])$ http://$host/$1$2/ permanent;
	}
	
	error_page 404 /404.html;
	location = /40x.html {
	root /host/wwwroot/test; #你的站点路径
	charset on;
	}
	
	# redirect server error pages to the static page /50x.html
	#
	error_page 500 502 503 504 /50x.html;
	location = /50x.html {
	root /host/wwwroot/test; #你的站点路径
	charset on;
	}
	
	#将客户端的请求转交给fastcgi
	location ~ .*\.(php|php5|php4|shtml|xhtml|phtml)?$ {
	fastcgi_pass 127.0.0.1:9000;
	include /host/nginx/conf/fastcgi_params;
	}
	
	#网站的图片较多，更改较少，将它们在浏览器本地缓存15天
	location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
	{
	expires 15d;
	}
	
	#网站会加载很多JS、CSS，将它们在浏览器本地缓存1天
	location ~ .*\.(js|css)?$
	{
	expires 1d;
	}
	
	location /(WEB-INF)/ { 
	deny all; 
	}
	
	#设定日志格式
	log_format access '$remote_addr - $remote_user [$time_local] "$request" '
	'$status $body_bytes_sent "$http_referer" '
	'"$http_user_agent" $http_x_forwarded_for';
	#设定本虚拟主机的访问日志
	access_log /host/nginx/logs/down/access.log access; #日志的路径,每个虚拟机一个,不能相同
	server_name_in_redirect off;
	}
	
	
	
###3.
www_test1_com.conf和www_test2_com.conf,文件和上面的基本相同,具体的日志内容如下:
www_test1_com.conf如下:

	..........
	
	#设定日志格式
	log_format test1 '$remote_addr - $remote_user [$time_local] "$request" '
	'$status $body_bytes_sent "$http_referer" '
	'"$http_user_agent" $http_x_forwarded_for';
	#设定本虚拟主机的访问日志
	access_log /host/nginx/logs/test1/test1.log test1; #日志的路径,每个虚拟机一个,不能相同
	server_name_in_redirect off;
	}
	
	
	
	www_test2_com.conf如下:
	程序代码
	..........
	
	#设定日志格式
	log_format test2 '$remote_addr - $remote_user [$time_local] "$request" '
	'$status $body_bytes_sent "$http_referer" '
	'"$http_user_agent" $http_x_forwarded_for';
	#设定本虚拟主机的访问日志
	access_log /host/nginx/logs/test2/test2.log test2; #日志的路径,每个虚拟机一个,不能相同
	server_name_in_redirect off;
	}


参考资料：

*	[http://www.dearroy.com/linux/2013/06/20/install-lnmp-on-debian-7.html](http://www.dearroy.com/linux/2013/06/20/install-lnmp-on-debian-7.html)
*	[http://lnmp.org/install.html](http://lnmp.org/install.html)
*	[http://rtxbc.iteye.com/blog/883328](http://rtxbc.iteye.com/blog/883328)