---
title: MediaWiki隐藏index
date: 2016-03-06 21:12:14
---
#Apache 

- 在httpd.conf配置文件中加载mod_rewrite.so模块，将前面的'#'去掉，如果没有则添加这句话:


	#LoadModule rewrite_module modules/mod_rewrite.so
- 然后将httpd.conf中


	AllowOVerride None	#改为 All

#Mediawiki
- 修改配置文件（LocalSettings.php）

如果存在 $wgArticlePath 将原来的注释掉，然后在$wgScriptPath下添加：

	$wgArticlePath      = "/$1";
- 设置.htaccess文件

在mediawiki所在目录添加.htaccess文件：

	RewriteEngine On
	RewriteCond %{REQUEST_FILENAME} !-f
	RewriteCond %{REQUEST_FILENAME} !-d
	#如果要定向到Main_Page去掉下面行首的#
	#RewriteRule ^/*$ /wiki/index.php?title=Main_Page[L,QSA]
	RewriteRule ^(.+)$ /wiki/index.php?title=$1 [L,QSA]