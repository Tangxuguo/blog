---
layout: post
title: "前端观察：PJAX和NProgress"
tags: [前端]
---

看到两个非常有意思的插件，PJAX和NProgress

PJAX可以让你不离开当前页面完成到其他页面的跳转，NProgress可以在顶部显示一个页面加载进度条，两者是一对好基友

github地址：

1. [https://github.com/rstacruz/nprogress](https://github.com/rstacruz/nprogress)
2. [https://github.com/defunkt/jquery-pjax](https://github.com/defunkt/jquery-pjax)


PJAX具体介绍看以参考这两篇文章：

1. [PJAX的实现与应用](http://www.cnblogs.com/hustskyking/p/history-api-in-html5.html)
2. [pjax：ajax和pushState结合的js库](http://www.welefen.com/pjax-for-ajax-and-pushstate.html)

NProgress具体看看这个介绍：

1. [NProgress - 实现和 Youtube 一样的细长进度条](http://c7sky.com/nprogress.html)

安装和使用请参考官方github的示例

本来想再博客上部署一下PJAX和nprogress，奈何PJAX不是很好使，加载进去后我的评论没法加载，另外NProgress的进度条没法终止，所有放弃了，以后再折腾

所以现在的效果只有NProgress

##使用方法


1. 加载js库和样式

		<script src='nprogress.js'></script>
		<link rel='stylesheet' href='nprogress.css'/>
	
	里面的样式随便自己写
		
	我的配置
		
		/* Make clicks pass-through */
		/* Modified by Peiwen Lu @ 06 Nov 2013 */
		#nprogress {
		  pointer-events: none;
		  -webkit-pointer-events: none; }
		
		#nprogress .bar {
		  background: #f8645a;
		  position: fixed;
		  z-index: 100;
		  top: 0;
		  left: 0;
		  width: 100%;
		  height: 2px; }
		
		/* Fancy blur effect */
		#nprogress .peg {
		  display: block;
		  position: absolute;
		  right: 0px;
		  width: 100px;
		  height: 100%;
		  box-shadow: 0 0 10px #f8645a, 0 0 5px #f8645a;
		  opacity: 1.0;
		  -webkit-transform: rotate(3deg) translate(0px, -4px);
		  -moz-transform: rotate(3deg) translate(0px, -4px);
		  -ms-transform: rotate(3deg) translate(0px, -4px);
		  -o-transform: rotate(3deg) translate(0px, -4px);
		  transform: rotate(3deg) translate(0px, -4px); }
		/* Remove these to get rid of the spinner */
		#nprogress .spinner {
		  display: block;
		  position: fixed;
		  z-index: 100; }
		  @media screen and (max-width: 585px) {
		    #nprogress .spinner {
		      top: 5px;
		      right: 5px; } }
		#nprogress .spinner-icon {
		  border: solid 2px transparent;
		  border-top-color: #f8645a;
		  border-left-color: #f8645a;
		  border-radius: 50%;
		  -webkit-animation: spin 400ms linear infinite;
		  -moz-animation: spin 400ms linear infinite;
		  -ms-animation: spin 400ms linear infinite;
		  -o-animation: spin 400ms linear infinite;
		  animation: spin 400ms linear infinite; }
2. 绑定动作

	 由于没法用PJAX，所以只用比较低级的两个加载开始和结束时间,如果有PJAX可以从开始点击的时候统计
	 
		 <script >
		
		$(document).ready(function(){
		
		    NProgress.start();
		  
		});
		$(window).load(function(){
		    NProgress.done();
		});
		</script>
	
	备注：
		
	$(document).ready()当文档dom构建完成后触发
	
	$(window).load()当页面所有内容下载完成后触发