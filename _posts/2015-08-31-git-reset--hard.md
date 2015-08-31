---
layout: post
title: "git reset --hard找回误删的文件"
tags: [linux]
---
新建的项目还没commit

不小心使用了git reset --hard，数据全没了怎么办？

如果你一不小心git add . 了，还好，数据还在，只是名字丢了，

如果你没有commit有没有add的话，那几本找不回来了，重新写吧

恢复方法：

1. 运行git fsck得到一些dangling blobs

		dangling commit fafeade9f348b18f37835ab49dab40172efde693

2. 运行git fsck --lost-found

	在隐藏文件夹.git/lost-found/other会有你需要的所有文件

3. 运行下面这条bash语句会把所有的文件转成txt文件

		for blob in $(git fsck --lost-found | awk ‘$2 == “blob” { print $3 }’); do git cat-file -p $blob > $blob.txt; done

4. 然后根据文件的内容搜索，比如html标签，PNG图片头部，python的 import等等，反正文件名是回不来了
	
祝你好运，反正我的已经找回来了

其他一些有用的命令

保存所有文件名到一个文件

	git fsck --cache --unreachable $(git for-each-ref --format="%(objectname)") > allhashes


	git fsck --full
	git ls-files -d
	git count-objects
	
英文参考下面的文章

https://medium.com/@CarrieGuss/how-to-recover-from-a-git-hard-reset-b830b5e3f60c

中文

http://blog.csdn.net/cqbamboo/article/details/17997217
