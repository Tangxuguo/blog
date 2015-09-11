---
layout: post
title: "前端特效：让你的头像转起来"
tags: [前端]
---

最近看到个有意思的特效
鼠标放在头像上，头像会自动旋转，就像本博客的头像

好了废话不多说，上代码

要实现头像旋转，并不需要什么js代码，CSS3自带旋转属性

找到你的头像，在html页面添加ID号

	<a href="/index.html" id="avatar"></a>

在CSS文件中添加如下内容

1. 添加特效。这里设置为鼠标悬浮后旋转

		#avatar {
		  width: 120px;
		  height: 120px;
		  margin: 55px auto 35px;
		  background: url(/assets/img/avatar120.jpg);
		  border-radius: 50%;
		  box-shadow: 0 0 0px 4px white, 0 0 4px 4px #eee;
		  overflow: hidden;
		  display: block; }
		  #avatar:hover {
		    -webkit-animation: spin 4s linear infinite;
		    -moz-animation: spin 4s linear infinite;
		    -ms-animation: spin 4s linear infinite;
		    -o-animation: spin 4s linear infinite;
		    animation: spin 4s linear infinite; }
		  @media (-webkit-min-device-pixel-ratio: 2), (min-resolution: 192dpi) {
		    #avatar {
		      background: url(/assets/img/avatar120@2X.jpg);
		      -webkit-background-size: 120px 120px;
		      -moz-background-size: 120px 120px;
		      background-size: 100px 120px; } }
		      
2. 设置特效效果，平滑旋转360度
		      
		@-webkit-keyframes spin {
		  from {
		    -webkit-transform: rotate(0deg); }
		
		  to {
		    -webkit-transform: rotate(359deg); } }
		
		@-moz-keyframes spin {
		  from {
		    -moz-transform: rotate(0deg); }
		
		  to {
		    -moz-transform: rotate(359deg); } }
		
		@-o-keyframes spin {
		  from {
		    -o-transform: rotate(0deg); }
		
		  to {
		    -o-transform: rotate(359deg); } }
		
		@-ms-keyframes spin {
		  from {
		    -ms-transform: rotate(0deg); }
		
		  to {
		    -ms-transform: rotate(359deg); } }
		
		@keyframes spin {
		  from {
		    transform: rotate(0deg); }
		
		  to {
		    transform: rotate(359deg); } }


3. 想要旋转地更有特色

	设置旋转时间，原来是4s一次，无限旋转	，名字是spin，旋转速度是线性，当然你可以设置为[淡入淡出ease-in-out,自定义速度曲线cubic-bezier](http://www.w3school.com.cn/cssref/pr_animation-timing-function.asp)，或者其他属性值
	
	
		animation: spin 4s  ease-in-out infinite;
		animation: spin 4s  cubic-bezier(0.36,0.8,0.69,0.23) infinite;

	设置旋转每一帧花费的时间，用百分比来规定变化发生的时间，或用关键词 "from" 和 "to"，等同于 0% 和 100%。
	
	
		@keyframes spin {
			  0% {
			    transform: rotate(0deg); }
			
			  100% {
			    transform: rotate(359deg); } }
		

下面这个是一个更炫酷的旋转特效[http://jezenthomas.com/arcify/](http://jezenthomas.com/arcify/)

<p data-height="268" data-theme-id="0" data-slug-hash="CAHsk" data-default-tab="result" data-user="jezen" class='codepen'>See the Pen <a href='http://codepen.io/jezen/pen/CAHsk/'>Arcify</a> by Jezen Thomas (<a href='http://codepen.io/jezen'>@jezen</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>