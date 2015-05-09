---
layout: post
title: "为博客加上豆瓣读书的书单"
tags: [生活]
---

前段时间一直在找douban图书的js代码，一直没找到，今天心血来潮，翻了一下自己博客的js代码库，发现自带了douban api库，搜了一下，找到了[pexcn的主题](https://github.com/pexcn/Jekyll-Light)于是搞起来吧.

本人所用douban读书的api库，由于是js，所有可以放在任何语言的博客上，只要浏览器支持js就行，这里我把它放在了github上

[下载](http://peqiu.com/blog/public/js/douban.api.js)

开使干活

在你要呈现的页面加载js脚本，注意需要jquery，但要避免重复加载jquery，下面是我的文件路径，可以通过查看源码和终端窗口调试

	<script type="text/javascript"  src="/blog/public/js/jquery.min.js"></script>
	<script type="text/javascript"  src="/blog/public/js/douban.api.js"></script>
	
增加容器，用来呈现图书列表

	<div id="archives">
	  <div id="douban"></div>
	</div>
	
设计页面css风格

	<style>.main{padding-right:0;}.douban-title{color:#1BAF69;margin-bottom:-20px;text-shadow:0 0 1px;letter-spacing:3px;}.douban-list{margin-left:-40px;}.douban-list ul li{list-style:none outside none;width:115px;margin-top:20px;float:left;}.douban-list ul li img{width:90px;height:120px;border:solid 1px #DDD;padding:1px;}.clear{clear:both;}@media (max-width:875px){.douban-title{letter-spacing:1px;margin-bottom:-15px;}.douban-list{margin-left:-37px;}.douban-list ul li{width:100px;margin-top:5px;}}</style>
	
初始化

	<script>
	var dbapi=new DoubanApi();$(document).ready(function(){dbapi.show()});
	</script>

然后更改刚才的api库中的配置，把user改成自己的用户名，去豆瓣申请一个api，选上图书，在这里填上，貌似不需要通过审核就可以用

	function DoubanApi() {
		this.defaults = {
			place:"douban",
			user:"54366967",
			api:"0dcabc8a7a7624272960359f6f626c75",
			book:[{status:"reading",maxnum:20},{status:"read",maxnum:100},{status:"wish",maxnum:100}],
			bookreadingtitle:"在读...",
			bookreadtitle:"读过...",
			bookwishtitle:"想读..."
		};
	}
	
	DoubanApi.prototype.make_api_url = function(type,user,key,status,begin,end) {
		var url = "http://api.douban.com/people/" + user + "/collection?cat=" + type 
			+ "&start-index=" + begin + "&max-results=" + end + "&status=" + status 
			+ "&alt=xd&callback=dbapi." + type + status + "_show";
		if (key.lenght > 0) {
			url += "&apikey=" + key;
		}
		return url;
	}
	
	DoubanApi.prototype.make_list_item = function(items) {
		var html = '';
		$.each(items,function(i,item){
			html += '<li><a href="'
				+ item.link + '" target="_blank"><img src="'
				+ item.src + '" alt="' + item.title
				+ '" title="' + item.title + '" /></a></li>';
		});
		return html;
	};
	
	DoubanApi.prototype.parse_json = function(json) {
		var items = [];
		$.each(json.entry,function(i,item) {
			var link = {};
			link.title = item["db:subject"]["title"]["$t"];
			link.link = item["db:subject"]["link"][1]["@href"];	//硬编码
			link.src = item["db:subject"]["link"][2]["@href"];	//硬编码
			items.push(link);
		});
		return items;
	};
	
	DoubanApi.prototype.fix_num = function(num) {
		var index = 1;
		var fixnums = [];
		if (50 > num && num  > 0) {
			fixnums.push({begin:index,end:num});
		}
		else {
			while (num > 0) {
				fixnums.push({begin:index,end:index + 49});
				num -= 50;
				index += 50;
			}
		}
		return fixnums;
	};
	
	DoubanApi.prototype.show = function() {
		var books = [];
		var tmpthis = this;
		$.each(this.defaults.book,function(i,item) {
			var fixnums = tmpthis.fix_num(item.maxnum);
			books.push({status:item.status,indexs:fixnums});
		});
	
		$.each(books,function(i,item) {
			$.each(item.indexs,function(t,idx) {
				tmpthis.appendScript(tmpthis.all_url("book",item.status,idx.begin,idx.end));
			});
		});
	};
	
	DoubanApi.prototype.appendScript = function(url) {
		if (url && url.length > 0) {
			$("<script/>").attr("src",url).attr("charset","utf-8").appendTo($("head")[0]);
		}
	};
	
	DoubanApi.prototype.all_url = function(type,status,begin,end) {
		if (end === 0 ) return;
		if (!this[type + status + "_show"]) {
			this[type + status + "_show"] = function(json) {
				var mainplace = $("#" + this.defaults.place);
				if (mainplace.length === 0) {
					mainplace = $('<div id="' + this.defaults.place + '"></div>').prependTo($("body"));
				}
				if ($("#" + type + status).length === 0) {
					var title = this.defaults[type + status + "title"];
					$('<h4 class="douban-title">' + title + '</h4>').appendTo(mainplace);
					$('<div id="' + type + status + '" class="douban-list"><ul></ul></div>').appendTo(mainplace);
					$('<div class="clear"></div>').appendTo(mainplace);
				}
				$("#" + type + status + " > ul").append(this.make_list_item(this.parse_json(json)));
			};
		}
		return this.make_api_url(type,this.defaults.user,this.defaults.api,status,begin,end);
	};
	
	
OK大功告成


整个代码看起来应该是这样的：

	<script type="text/javascript"  src="/blog/public/js/jquery.min.js"></script>
	<script type="text/javascript"  src="/blog/public/js/douban.api.js"></script>
	<style>.main{padding-right:0;}.douban-title{color:#1BAF69;margin-bottom:-20px;text-shadow:0 0 1px;letter-spacing:3px;}.douban-list{margin-left:-40px;}.douban-list ul li{list-style:none outside none;width:115px;margin-top:20px;float:left;}.douban-list ul li img{width:90px;height:120px;border:solid 1px #DDD;padding:1px;}.clear{clear:both;}@media (max-width:875px){.douban-title{letter-spacing:1px;margin-bottom:-15px;}.douban-list{margin-left:-37px;}.douban-list ul li{width:100px;margin-top:5px;}}</style>
	<div id="archives">
	  <div id="douban"></div>
	</div>
	<br/><br/>
	
	<script>
	var dbapi=new DoubanApi();$(document).ready(function(){dbapi.show()});
	</script>

[最后效果戳](http://peqiu.com/blog/pages/read.html)

