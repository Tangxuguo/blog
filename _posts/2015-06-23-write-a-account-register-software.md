---
layout: post
title: "如何写一个网站账号批量注册软件"
tags: [网络编程]
---
如何写一个网站账号批量注册软件，一般的网站在防止用户恶意注册的时候采用验证码，IP限制，cookie限制，那么如何突破这些限制呢？下面一一道来
##模拟登陆
要实现批量注册，首先你要解决的第一个问题是能够模拟登陆

这里用到的一个python库-[requests](http://docs.python-requests.org/en/latest/)，正如它的介绍--Requests: HTTP for Humans，设计非常人性化

requests可以模拟正常浏览器的访问，设置cookies，sessions，这样可以突破cookie限制

	1. 首先我们使用requests访问登陆页面并且保持sessions
	2. 获取登陆页面表单
		打开你的chrome浏览器，审查元素，找到网络选项卡，在登陆页面先登陆一次，看看提交了什么，提交到了哪个地址
	3. 然后构造表单内容，使用requests提交，

这里有些网站可能有验证码限制，这里我们先手动验证，稍后再介绍如何crack

使用requests拉取一次验证码，保存为图片，人眼识别，手动输入

看看返回内容，是否是登陆成功的页面

这里以http://www.yiguinfo.com/ 网站为例分析

首先需要分析表单内容

获取表单post地址
<img src="/blog/public/images/posts/register/post_url.png" >

获取表单内容
<img src="/blog/public/images/posts/register/form.png" >

获取表单验证码地址
<img src="/blog/public/images/posts/register/auth_img.png" >

完整的代码如下

	import re
	import sys
	import requests
	from PIL import Image
	from StringIO import StringIO	
	
	def Login_Site(username,password):
	
	    Site_login = r"http://www.yiguinfo.com/heistudy/web/loginAction.action"
	    Site_url = r"http://www.yiguinfo.com/"
	    auth_image_url = r"http://www.yiguinfo.com/captchaImageServlet.servlet"
	    
	    global Site_status
	    global Site_session
	    
	    Site_session = requests.Session() #用requests设置cookie
	    headers = {
	        "User-Agent":"Mozilla/5.0 (Windows NT 6.1; WOW64; rv:24.0)\
	        Gecko/20100101 Firefox/24.0",
	        "Referer":"http://www.yiguinfo.com/"
	            }
	
	    login = Site_session.get(Site_url)
	    Site_status = "Login Failure"
	    #登录成功会出现下面字符
	    login_status = "glyphicon glyphicon-user"
	    while (login.status_code != 200 or Site_status == "Login Failure"):
	        image_name = "auth_img.jpg"
	        auth_img = Site_session.get(auth_image_url)
	        i = Image.open(StringIO(auth_img.content))
	        i.save(image_name)
	
	        auth_code = raw_input("please input auth code")
	        #提交账号密码
	        logininfo = {'submitToken':'',
	                    'currentAppId':101,
	                    'mainAppId':101,
	                    'username':username,
	                    'pswd':password,
	                    'authcode':auth_code}
	        #post登录网站
	        login = Site_session.post(Site_login,data=logininfo, headers=headers, timeout=10)
	        if re.search(login_status, login.text) != None:#判断登录状态
	            print ("Login Success")
	            Site_status = "Login Success"
	        else:
	            print ("Login Failure")
	            Site_status = "Login Failure"
	
	    return  Site_status

##代理IP
为什么要使用代理IP？

因为有些网站是有IP限制的，即一个IP只能在网站上注册一个或几个，多了就被封了，另一方面为了安全着想，你也不想干坏事被抓住吧

获取方法：
目前市面上有很多网站提供免费和付费代理，有钱就去买吧，当然你也可以去
找免费代理如[国内高匿代理IP](http://www.xici.net.co/nn/),[鲲鹏免费HTTP代理列表](http://www.site-digger.com/html/articles/20110516/proxieslist.html),[快代理](http://www.kuaidaili.com/free/)等等

这里试了http://www.xici.net.co/nn/这个网站的代理，还不错

写个爬免费代理的，Beautiful Soup 是一个Python 写的处理HTML /XML的模块，[中文版说明文档](http://www.crummy.com/software/BeautifulSoup/bs3/documentation.zh.html)，借助于这个模块来分析页面，这里我们至提取了页面的IP地址，端口号，协议类型，如果需要其他的可以自己提取

	def Get_Proxies():
	
	    global Proxies_Site_session
	    Proxies_List=[]
	
	    Proxies_Site_url =  r"http://www.xici.net.co/nn/"
	    Proxies_Site_session = requests.Session() #用requests设置cookie
	    headers = {
	        "Accept":"text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8",
	        "User-Agent":"Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_3) \
	        AppleWebKit/537.36 (KHTML, like Gecko) Chrome/43.0.2357.124 Safari/537.36",
	        "Referer":"http://www.xici.net.co/"
	            }
	    r = Proxies_Site_session.get(Proxies_Site_url,headers=headers)
	    if r.status_code == 200 :
	        soup = BeautifulSoup(r.text)
	        soup_list=soup.find("table",attrs={"id":"ip_list",}).findAll("tr")[1:-1]
	        for soup_list_i in soup_list:
	            Proxies = []
	            Proxies.append(soup_list_i.findAll("td")[2].string)
	            Proxies.append(soup_list_i.findAll("td")[3].string)
	            Proxies.append(soup_list_i.findAll("td")[6].string)
	            Proxies_List.append(Proxies)
	    else :
	        print r.status_code
	    return Proxies_List


返回：

	[[u'218.204.140.105', u'8118', u'HTTP'], [u'120.203.159.19', u'8118', u'HTTP'], [u'118.189.79.43', u'80', u'HTTPS'],...]


使用：

爬到代理后，使用requests的代理设置功能，轻轻松松解决代理IP访问问题
官方教程

	import requests
	
	proxies = {
	  "http": "http://10.10.1.10:3128",
	  "https": "http://10.10.1.10:1080",
	}
	
	requests.get("http://example.org", proxies=proxies)

这里我们改一下，只使用http代理

	Proxies_List = Get_Proxies()
    proxies = {}
    i=0
    while (Proxies_List[i][2]!="HTTP"):
        i += 1
    proxies = {
        "http": "http://"+Proxies_List[i][0]+":"+Proxies_List[i][1],
        }


##账号字典

为什么需要一个账号字典呢？

因为我们需要一个像人类使用的用户名，你也不想你注册的名字像test0001，test0002，test0003，test0004，这么老土吧

1. 用户名
	最近很多网站泄露出来了很多用户账户数据库，比如CSDN600万，猫扑1000万，找一个数据库，获取到用户名和密码，作为我们的账号字典，
	
	下载地址请谷歌
	
	导入方法谷歌到的
	
	[某社区600万用户数据导入MYSQL、MSSQL、Oracle数据库方法](http://www.cnblogs.com/xwdreamer/archive/2012/06/08/2541678.html)
	
	在这里我们使用Mysql，使用Myisam引擎，UTF8编码，新建一个数据库名字为user，在其中建一个csdnuser表
	

		CREATE TABLE `csdnuser` (
		 `id` int(11) NOT NULL AUTO_INCREMENT,
		 `username` varchar(255) DEFAULT NULL,
		 `password` varchar(255) DEFAULT NULL,
		 `email` varchar(255) DEFAULT NULL,
		 PRIMARY KEY (`id`)
		) ENGINE=MyISAM DEFAULT CHARSET=utf8;
		
	导入数据方法
	
		load data local infile 'd:\\www.csdn.net.sql' into table csdnuser2 fields terminated by ' # ' (username,password,email);

	
2. 邮箱验证
	这个问题有点麻烦
	如果你要注册的网站不使用邮箱认证的话还好，直接填上面数据库中的邮箱，如果使用了邮箱的话，就要想办法了
	1. 采用自己的域名，可以生成无数的邮箱，自己控制，这种方法一般很容易被发现
	2. 另一种方法就是去批量注册163，sina等邮箱，这种方式一般难度较大，那些邮箱验证码也是很难破的
	3. 还有一种方式就是用钱，淘宝上买吧
3. 姓名和身份证
	这个一般不需要，遇到一些需要实名认证的可能需要
	解决办法，目前网上有很多流传出来的姓名和身份证，可以爬一下
	

##验证码破解

到了最后一步，如何破解验证码

目前网上可选的方案有两个:
	
1. 用付费的人工验证码破解：
	1. 国内有很多打码平台如联众打码平台 (每 1000 个验证码 ￥10 )，慧眼答题平台 (每 1000 个验证码 ￥12 )，官方都提供了部分接入API，价格也差不多
	2. 国外出名的有antigate 提供的人肉识别服务 (最低每1000个验证码 $0.7)这里有一个[antigate wrapper](https://github.com/gotlium/antigate)

2. 自己用算法做识别：
但是这个验证码不同网站难度不一样，
	1. 简单的可以使用基于google的光学字符识别Tesseract-OCR和Cuneiform的[PyOCR](https://github.com/jflesch/pyocr)
	2. 复杂的可以使用基于SVM算法的[captchacker](https://github.com/bratao/captchacker)
		
		

