# LAMP编译安装

LAMP指的是Linux Apace Mysql PHP

## Linux

本文使用的是最小化安装的centos7，在安装之前先查看系统是否安装了PHP,Mysql,Apache.查看是否安装

`rpm -q http mysql php`

如果有安装请卸载

`rpm -e httpd —nodeps`

`rpm -e mysql —nodeps`

`rpm -e php —nodeps`

在安装其他软件之前要先安装必要环境库。

查看是否安装环境库 

`rpm -q make gcc gcc-c++ zlib-devel libtool libtool-ltdl libtool-ltdl-devel bisonncurses-devel`

没有的话先安装环境库

`yum -y install make gcc gcc-c++ zlib-devel libtool libtool-ltdllibtool-ltdl-devel bison ncurses-devel libaio`

安装zlib(数据压缩用的函数库)从[官网](http://www.zlib.net)下载最新版，本次下载的是最新版zlib-1.2.8.tar.gz，进入文件目录。

`tar -zxvf zlib-1.2.8.tar.gz`

`cd zlib-1.2.8`

`./configure`

`make && make install`

如果安装成功将会在/usr/local/zlib目录下生成include,lib,share三个目录。

## Apache

同样从apache[官网](http://apache.claz.org/httpd/)下载最新版，本次下载的是最新版httpd-2.4.23.tar.bz2，进入文件目录。

`tar -jxvf httpd-2.4.23.tar.bz2`

`cd httpd-2.4.23`

`./configure --prefix=/usr/local/http2 --enable-modules=all --enable-mods-shared=all --enable-so`

### 配置文件解析：

`--prefix=/usr/local/http2`     apache安装位置

`--enable-modules=all`	 开启全部功能模块

`--enable-mods-shared=all`      

功能模块与Apache软件的集成关系为共享型

* 共享型：

  ​	把功能模块对应的程序代码都编译进apache软件内部。造成的结果：apache软件本身稍显臃肿，运行速度少慢，但是apache调用功能模块的速度较快。优势体现在频繁大量使用功能模块。

* 独立类型:

  ​        功能模块代码与apache软件分离处理。apache需要功能模块支持的时候才引入进来。造成的结果：apache软件本身非常小巧、运行速度快。apache频繁、大量代用功能模块时候速度较慢。优势体现在很少使用功能模块情况。

`--enable-so`      使得apache可以识别so后缀的功能模块文件

如果没有报错执行编译和安装，如果报错，根据报错解决问题

`make && make install`

安装完成测试apache服务，启动apache

`/usr/local/http2/bin/apachectl start`

浏览器打开：http://虚拟机IP，看到"it works"，即为启动apache成功。

## PHP

在编译PHP之前，我们要安装依赖库。分别下载以下依赖库。

- [libxml2]()
- [libmcrypt]()
- [jpeg]()
- [libpng]()
- [libgd]()
- [freetype]()

### libxml2

libxml2是一个xml的c语言解析器，支持C ,c++,ph,ruby,Pascal,tcl绑定

`tar -zxvf libxml2-.tar.gz`

`cd libxml2`

`./configure --prefix=/usr/local/libxml2/`

`make && make install`

安装成功以后，在/usr/local/libxml2/目录下查看是否生成bin,include,lib,share四个目录。

### libmcrypt

libmcrybt是加密算法扩展库

`tar -zxvf libmcrypt-tar.gz`

`cd libmcrypt`

`./configure --prefix=/usr/local/libmcrypt/`

`make && make install`

安装成功以后，在/usr/local/libmcrypt/目录下生成bin,include,lib,man,share五个目录。

安装完成libmcrypt库以后，不同的linux系统版本有可能还要安装一下libltdl库。安装方法和前面的步骤相同，可以进入到解压缩的目录下，找到libltdl库源代码所在的目录，按照下面几个命令配置、编译、安装就可以了。

`./configure —enable-ltdl-install`

`make && make install`

### jpeg

安装GD2库前所需要的jpeg8库

### libpng

同样是安装GD2库所需要的png库

`tar zxvf libpng-1.tar.gz`

`cd linpng`

`./configure —prefix=/usr/local/libpng`

`make && make install`

同样的如果安装成功将会在/usr/local/libpng目录下生成bin,include,lib和share四个目录。

### freetype

同样是安装GD库需要的字体库

`tar zxvf freetype-2..tar.gz`

`cd freetype-`

`./configure --prefix=/usr/local/freetype`

`make && make install`

同样的如果安装成功将会在/usr/local/freetype目录下生成bin,include,lib和share四个目录。

### GD

`tar zxvf libgd-.tar.gz`

`cd libgd`

`./configure --prefix=/usr/local/gd --with-jpeg=/usr/local/jpeg/ --with-png=/usr/local/png --with-freetype=/usr/local/freetype --with-zlib `

`make && make install`

如果安装成功会在/usr/local/gd目录下生成bin,include和bin这三个目录。

### curl

`sudo yum install libcurl-devel`

### OpenSSL

`sudo yum install openssl-devel`

### PHP

`tar `

`cd `

`./configure --prefix=/usr/local/php --enable-opcache --enable-fpm  --with-apxs2=/usr/local/apache2/bin/apxs --with-gd=/usr/local/gd --with-zlib  --with-jpeg-dir=/usr/local/jpeg --with-png-dir=/usr/local/libpng --with-freetype-dir=/usr/local/freetype --with-pdo-mysql=mysqlnd --enable-mbstring --enable-sockets --with-curl --with-mcrypt --with-openssl `

`make && make install`

如果有报错，根据报错信息解决。

#### 配置文件解析

`--prefix=/usr/local/php`PHP安装目录

`--enable-opcache`启用PHP内置的字节码缓存系统

`--enable-fpm`启用PHP内置的FastCGI进程管理器

`--with-apxs2`apache支持参数

`--with-pdo-mysql`让PHP使用原生的MySQL驱动为MySQL数据库启动PDO数据库抽象API

`--enable-mbstring`让PHP支持多子节(Unicode)字符串

`--enable-sockets`让PHP支持网络套接字，以便通过TCP套接字与远程设备通信

`--with-curl`让PHP与操作系统的curl库交互，以便使用PHP中的curl函数收发HTTP请求

`--with-mcrypt`让PHP与操作系统的mcrypt库交互，用户加密和解密数据

`--with-openssl`让PHP与操作系统中的openssl库交互，使用PHP的HTTPS流封装协议

安装完成复制php.ini配置文件到指定目录

`cp php.ini-development /usr/local/php/etc/php.ini`

编辑php.ini

`vim php.ini`找到date.timezone,改成date.timezone="PRC"。

更改apache文件，解析php

`vim /etc/httpd/httpd.conf`在LoadModule模块中添加(如果存在就跳过)LoadModule php5_module modules/libphp5.so

检查文件是否存在

`cd /usr/local/apache2/modulesls -al`看看有没有libphp5.so这个文件

编辑apache配置文件

`vim /etc/httpd/httpd.conf`找到AddType application/x-compress .ZAddType application/x-gzip .gz .tgz 在下面添加AddType application/x-httpd-php .php .phtml AddType application/x-httpd-php-source .phps找到DirectoryIndex index.html修改为DirectoryIndex index.php index.html index.htm

重启apache

`service httpd stopservice httpd start`

验证apache支持php

进入apache服务器网站跟目录`cd /usr/local/apache2/htdocs/`。

`vim test.php`写入`<?php info(); ?>`保存并退出。

浏览器访问。

# MySQL

安装MySQL之前要先安装cmake，先去[官网]()下载cmake.

`tar zxvf cmake.tar.gz`

`cd cmake`

`./boostrap`

`gmake`

`make && make install`

设置用户，组和目录

`sudo groupadd mysql`创建mysql组

`usradd mysql -r -g mysql`给mysql用户分组

`mkdir /usr/local/mysql`安装目录

`mkdir /usr/local/mysql/data`数据仓库目录

`chown -R mysql.mysql /usr/local/mysql`分配权限

安装MySQL

`tar zxvf mysql-.tar.gz`

`cd mysql-`

`cmake -DCMAKE-INSTALL_PREFIX=/usr/local/mysql -DMYSQL_UNIX_ADDR=/usr/local/mysql/mysql.lock -DDEFAULT`