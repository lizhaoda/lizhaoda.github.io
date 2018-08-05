---
title: centos上安装wordpress
url: 55.html
id: 55
categories:
  - 那些年玩的东西
date: 2016-08-09 13:11:57
tags:
---
本文是在centos7的 LAMP环境下搭建的wordpress！

 LAMP环境就是Linux+Apache+Mysql+PHP。因此目前Mysql被MariaDB所代替。MariaDB的目的是完全兼容MySQL，包括API和命令行，使之能轻松成为MySQL的代替品
## 安装Apache Web服务器
```
sudo yum install httpd
```
安装完成之后我们就可以运行以下命令启动Apache服务器了：
```
sudo systemctl start httpd.service
或者sudo service httpd start
```
之后我们就可以在浏览器中打开http://your_server_IP_address/ 我们新安装的网站，检查一下Apache是否安装成功，正常运行。
有时候我们可能打不开，原因是防火墙限制了外包访问，我们开启再试试看防火墙：
```
sudo iptables -I INPUT -p TCP --dport 80 -j ACCEPT
```
或者这样，防火墙放行http和https协议：
```
sudo firewall-cmd --permanent --zone=public --add-service=http
sudo firewall-cmd --permanent --zone=public --add-service=https
sudo firewall-cmd --reload
```
这次可以正常访问了：
如果想以后重启服务器之后自动启动Apache服务器，可以运行以下命令：
```
sudo systemctl enable httpd.service
或者sudo chkconfig httpd on
```
> Apache服务器的网站文件默认在/var/www目录。

## 安装Mysql(MariaDB)数据库
运行以下命令安装MariaDB数据库：
```
sudo yum install mariadb-server mariadb
```
完成之后启动数据库：
```
sudo systemctl start mariadb
```
然后安装一个数据库安全脚本，去掉一些危险的默认设置：
```
sudo mysql_secure_installation
```
然后会提示你输入数据库的**root账号密码（下面的登陆数据库和配置的时候要用）**，如果是新安装的则输入空格，如下所示：
```
In order to log into MariaDB to secure it, we'll need the current
password for the root user. If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.
Enter current password for root (enter for none):
```
然后我们输入空格，继续设置root密码：
下面所有的均为Y！
```
Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.
Set root password? [Y/n] Y
New password:
Re-enter new password:
Password updated successfully!
Reloading privilege tables..
... Success!
By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them. This is intended only for testing, and to make the installation
go a bit smoother. You should remove them before moving into a
production environment.
Remove anonymous users? [Y/n]
... Success!
Normally, root should only be allowed to connect from 'localhost'. This
ensures that someone cannot guess at the root password from the network.
Disallow root login remotely? [Y/n]
... Success!
By default, MariaDB comes with a database named 'test' that anyone can
access. This is also intended only for testing, and should be removed
before moving into a production environment.
Remove test database and access to it? [Y/n]
- Dropping test database...
... Success!
- Removing privileges on test database...
... Success!
Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.
Reload privilege tables now? [Y/n]
... Success!
Cleaning up...
All done! If you've completed all of the above steps, your MariaDB
installation should now be secure.
Thanks for using MariaDB!
```
同样的，设置开机自动启动MariaDB数据库：
```
sudo systemctl enable mariadb.service
或者sudo chkconfig mysqld on
```
> 或者我们可以安装 MySql，安装 MySql，并启动 MySql
```
sudo yum install mysql-server
sudo service mysqld start
```
通过上面的命令就可以安装并启动了 mysql，安装 mysql 的时候会询问你一些简单的配置，输入 enter 和 yes 一路下来就 OK 了。

另外，注意
> Centos 7 comes with MariaDB( MariaDB数据库管理系统是MySQL的一个分支) instead of MySQL. MariaDb is a open source equivalent to MySQL and can be installed with
```
yum -y install mariadb-server mariadb.
```
If you must have mysql you need to add the mysql-community repo
```
sudo rpm -Uvh http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
```
and then you can install MySQLl like you normally do.

## 安装PHP
运行以下命令安装PHP：
```
sudo yum install php php-mysql
```
安装完成，重启以下Apache服务器：
```
sudo systemctl restart httpd.service
```
PHP安全完成之后，我们可以在网站目录下面建立一个info.php的文件来查看php的安装情况我们在/var/www/html目录创建一个info.php的文件：
```
sudo vi /var/www/html/info.php
```
其info.php内容如下：
```
<?php phpinfo(); ?>
```
我们我们安装PHP成功，浏览器打开http://your_server_IP_address/info.php 将会看到php内容。

> 安装 PHP 相关组件，实际情况不安装也行。
```
yum install php-gd php-imap php-ldap php-odbc php-pear php-xml php-xmlrpc
```
我先安装了这几个组件，为以后使用，你要想了解所有的 PHP 组件的话，可以使用如下命令搜索：
```
yum search php-
```

## 安装phpMyAdmin
phpMyAdmin是个管理MariaDB数据库的Web界面程序，我喜欢装一个。

我们首先安装EPEL库，这个库提供很多额外的软件包：
```
sudo yum install epel-release
```
完成之后直接安装phpMyAdmin：
```
sudo yum install phpmyadmin
```
完成之后，我们设置phpMyAdmin的httpd设置/etc/httpd/conf.d/phpMyAdmin.conf：
```
Alias /phpMyAdmin /usr/share/phpMyAdmin
Alias /phpmyadmin /usr/share/phpMyAdmin
<Directory /usr/share/phpMyAdmin/>
  AddDefaultCharset UTF-8
  <IfModule mod_authz_core.c>
  # Apache 2.4
  <RequireAny>
  Require ip 127.0.0.1
  Require ip ::1
  </RequireAny>
  </IfModule>
  <IfModule !mod_authz_core.c>
  # Apache 2.2
  Order Deny,Allow
  Deny from All
  Allow from 127.0.0.1
  Allow from ::1
  </IfModule>
</Directory>
```
从配置中可以看出，可以用http://115.159.203.85/phpmyadmin 去访问phpMyAdmin。实际上我们在浏览里打开这个地址是403 Forbidden，这是因为还有权限控制,我们更改一下权限：
```
<Directory /usr/share/phpMyAdmin/>
  AddDefaultCharset UTF-8
  <IfModule mod_authz_core.c>
  # Apache 2.4
  <RequireAny>
  #Require ip 127.0.0.1
  #Require ip ::1
  Require all granted
  </RequireAny>
  </IfModule>
  <IfModule !mod_authz_core.c>
  # Apache 2.2
  Order Deny,Allow
  # Deny from All
  # Allow from 127.0.0.1
  # Allow from ::1
  Allow from All
  </IfModule>
</Directory>
```
然后再重启以下Apache服务器：
```
sudo systemctl restart httpd.service
```
在浏览器输入http://server_domain_or_IP/phpMyAdmin ，可以看到管理界面：

然后可以使用**数据的root密码登录**了。

## 安装Wordpress
创建数据库

我们先要创建Wordpress的数据库：
**用户名字为root，密码为zhaoda611，这个会在下面的wp-config.php配置的时候用到 ！**
```
# 登录数据库
mysql -u root -p
# 创建数据库
CREATE DATABASE wordpress;
drop database wordpress;
show databases;
```
> 以下仅供参考
```
# 创建数据库用户和密码
CREATE USER wordpressuser@localhost IDENTIFIED BY 'wordress_password';
# 设置wordpressuser访问wordpress数据库权限
GRANT ALL PRIVILEGES ON wordpress.* TO wordpressuser@localhost IDENTIFIED BY 'wordress_password';
# 刷新数据库设置
FLUSH PRIVILEGES;
# 退出数据库
exit
```

我们先下载wordpress安装包：
```
cd ~
wget http://wordpress.org/latest.tar.gz
或者wget http://wordpress.org/latest.zip
```

然后解压出来，拷贝到/var/www/html/wordpress目录：
```
# 解压wordpress
tar xzvf latest.tar.gz
或者unzip latest.zip
# 拷贝到/var/www/html/wordpress目录，或者html的根目录
sudo rsync -avP ~/wordpress/ /var/www/html/wordpress/
或者cp -rf wordpress/* /var/www/html/
    \cp -rf wordpress/* /var/www/html/不提示直接全部覆盖复制
```

> 以下仅供参考！实际只要输入ip地址，按照要求配置即可
然后编辑wp-config.php文件：
```
# 切换到wordpress目录
cd /var/www/html/wordpress
# 复制wp-config.php文件
cp wp-config-sample.php wp-config.php
# 编辑wp-config.php文件
sudo vim wp-config.php
```
然后在配置文件里设置正确的值：
```
// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
/*define('DB_NAME', 'database_name_here'); */
define('DB_NAME', 'wordpress');
/** MySQL database username */
/*define('DB_USER', 'username_here');*/
define('DB_USER', 'root');
/** MySQL database password */
/*define('DB_PASSWORD', 'password_here');*/
define('DB_PASSWORD', 'zhaoda611');
/** MySQL hostname */
define('DB_HOST', 'localhost');
```

另外，以下仅供参考
**配置Apache(一般来说安装完Apache之后就会自动生成/var/www/html文件夹)**

首先用Vim编辑器打开Apache的配置文件
```
vim /etc/httpd/conf/httpd.conf
```
一般来说没有什么需要特别改动的地方，我们只需要在最后面加一下自己的网站就行了，假设我们的网站文件放在了“/var/www/html/default”文件夹下。那我们只需要在Apache的配置文件最后面添加这么一段
```
<VirtualHost *:80>
ServerName localhost
DocumentRoot /var/www/html/default
<Directory “/var/www/html/default”>
Options FollowSymLinks
AllowOverride None
Order allow,deny
Allow from all
</Directory>
</VirtualHost>
```
然后保存退出。
然后重启Apache服务使配置文件生效
```
systemctl restart httpd.service
```