# centos7安装lnmp教程

全篇采取yum安装的方式，安装之前，要先把SELinux关闭了，要不然会导致nginx权限问题，修改配置文件。

`sudo vim /etc/selinux/config`

将SELINUX=enforcing改为SELINUX=disabled即可。

## php

由于采用在线安装php的方式，但是Centos和RHEL没有在默认的软件仓库中提供最新稳定版的PHP。所以我们要添加两个仓库。

添加EPEL仓库

`yum install epel-release`

添加remi仓库

`sudo rpm -Uvh https://mirrors.tuna.tsinghua.edu.cn/remi/enterprise/remi-release-7.rpm`

添加完成后，检查/etc/yum.repos.d/目录中是否有epel.repo和remi.repo两个文件。

然后安装php5.6

`yum --enablerepo=epel,remi,remi-php56 install php-fpm php-cli php-gd php-mbstring php-mcrypt php-mysqlnd php-mysql php-mysqli php-opcache php-pdo php-devel`

安装完成先确认一下是否安装成功

`php -v`

接着配置php

`sudo systemctl restart php-fpm.service`

设置php-fpm开机启动

`sudo systemctl enable php-fpm`

## nginx

`sudo yum install nginx`

设置nginx开机启动

`sudo systemctl enable nginx.service`



## MySQL

下载安装官方的仓库

`sudo rpm -Uvh http://dev.mysql.com/get/mysql57-community-release-el7-7.noarch.rpm`

`sudo yum install mysql-community-server`

启动MySQL

`sudo systemctl start mysqld.service`



