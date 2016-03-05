---
title: 腾讯云CentOS 安装MediaWiki
date: 2016-03-05 16:34:19
---
参考 ： https://www.digitalocean.com/community/tutorials/how-to-install-mediawiki-on-centos-7

 

安装软件包：

	yum install httpd php php-mysql php-gd php-xml mysql-server mysql

#MySQL配置
启动mysql服务：
	service mysqld start
设置mysql：
	mysql_secure_installation
创建wiki要用的数据库，并赋予相关权限
	mysql -u root -p //登陆mysql

	//在mysql或者MariaDb中输入下列命令
	CREATE DATABASE wikidb;
	GRANT ALL PRIVILEGES ON wikidb.* TO 'wikiuser'@'localhost' IDENTIFIED BY 'password';

	//如果你的数据库和服务器没有运行在同一个主机中，需要使用下面的语句
	GRANT ALL PRIVILEGES ON wikidb.* TO 'wikiuser'@'mediawiki.example.com' IDENTIFIED BY 'password';

*注：wikiuser为用户名, wikidb_passwd为该用户的数据库密码，建议修改。

至此，数据库配置完成。

#Apache配置

需要先修改httpd.conf文件

	vim /etc/httpd/conf/httpd.conf
将 #ServerName www.example.com:80前面的#去掉

Vim 中命令模式下" /" 向前查找字符， "?" 向后查找字符；

将图中的汉字部分，修改成为拥有的域名或者IP地址

修改PHP的配置文件： 
	
	vim /etc/php.ini

将overload的值修改为0.即关闭状态。如果不做修改且没有配置PHP的cache软件，后面打开网页配置时提示有错误。

*注：如果使用PHP的cache软件，例如Xcache可以开启此项。

重启http服务：

	service httpd restart
##防火墙配置
防火墙的配置

由于需要开放80端口供外界访问，我们需要对防火墙进行相应的配置。

	vim /etc/sysconfig/iptables
在其中加入一行规则：

	-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
而后重启防火墙

	/etc/init.d/iptables restart
*注：这里不是必须设置，如果没有做限定，就不用修改

确认Apache 和 MySQL开机时启动

	chkconfig httpd on

	chkconfig mysqld on

#Mediawiki的安装
这里手动下载安装包：

	wget https://releases.wikimedia.org/mediawiki/1.23/mediawiki-1.23.13.tar.gz

	https://releases.wikimedia.org/mediawiki/1.26/mediawiki-1.26.2.tar.gz
如果不能安装： 	
	yum install wget

解压包到当前目录

	tar xf mediawiki-1.23.13.tar.gz
创建目标文件夹，存放mediawiki的web页面内容：
(也可以解压到网站根目录)

	mkdir -p /var/www/html/wiki/
	chmod 777 /var/www/html/wiki
*这里为了方便，直接给了该目录全部读写权限

进入存放web内容的目录，并复制文件到目标目录下

	cd mediawiki-1.23.13
	cp * /var/www/html/wiki/
通过浏览器进行设置
浏览器中输入：bincoding.cn/wiki/index.php

*注：上面ip可以根据httpd.conf里面的设置来访问，/wiki/是上面创建的目录。