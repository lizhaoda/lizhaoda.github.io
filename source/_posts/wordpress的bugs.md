---
title: wordpress的bugs
url: 106.html
id: 106
categories:
  - 那些年玩的东西
date: 2016-07-30 06:02:49
tags:
---

有一次通过这个可以进入安装wordpress的界面！

  

http://115.159.203.85/wordpress/wp-admin/install.php

  

* * *

使用Wordpress程序架构的网站如果需要在网站后台升级、安装主题或者插件的时候，总是会提示需要我们提供FTP信息的界面。有这样的字样提示"要执行请求的操作，WordPress需要访问您网页服务器的权限。请输入您的FTP登陆凭据以继续。如果您忘记了您的登陆凭据(如用户名、密码)，请联系您的网站托管商"。这个是比较麻烦的，现在就尝试解决一下！

  

第一，如果我们安装的是lnmp一键安装包，那可以使用。授权组来解决。

  

chown -R www /home/wwwroot/www.yxlgh.com(修改成网站域名目录)

  

其中 www是www用户组 一定加上-R 递归指定。

  

第二，如果是其他的可以使用在wp-config.php文件中添加脚本方式。

  

define("FS_METHOD","direct");

  

define("FS\_CHMOD\_DIR", 0777);

  

define("FS\_CHMOD\_FILE", 0777);

  

上述脚本添加到文件最后面就可以。

其实这两个方法都是通用的，不存在那个好坏。

  

第三，更改用户权限：

  

 chmod -R 777 wp-content

  

第四，可以通过ftp传到wp-content下面的themes，plugins等目录实现

  

* * *

直接重新下载一个wordpress的zip包，解压之后步骤10，会进不去网站，这个时候重新安装mysql

删除Mysql

  

  yum remove mysql mysql-server mysql-libs mysql-server;

  find / -name mysql 将找到的相关东西rm掉；

  rpm -qa|grep mysql(查询出来的东东yum remove掉)

  

这个时候在重新安装会提示打开mysql失败，所以我们重新安装的时候必须参照上述的步骤二

  

* * *

禁用CentOS下Apache的测试页面

This configuration file enables the default "Welcome" page if there is no default index page present for the root URL.  To disable the  Welcome page, comment out all the lines below.

  

cd /etc/httpd/conf.d

vim welcome.conf   //文件中的说明性内容说明了该文件的主要作用，以及关闭该作用的方法。其实该文件也是一个普通的配置文件，并被包含进

了Apache服务器httpd.conf主文件中，只要用"#"将welcome.conf的内容注释掉即可

service httpd restart

  

* * *

需要注意的是，WordPress官方英文版不包含任何语言包，也就是你在 /wp-content/ 目录下看不到 languages 文件夹，即使你设置为 zh\_CN ，也不会生效，因为没有简体中文语言包！所以你必须下载对应语言的版本，解压后将 /wp-content/ 目录下 languages 文件夹（及其里面的文件）上传到你网站的 /wp-content/ 目录。WordPress 4.0 及以上版本，可直接在后台-设置-常规，设置“站点语言”，不再需要在 wp-config.php 定义 define('WPLANG', 'zh\_CN'); 啦！！

  

* * *

当网站所有的ip均没有办法访问（ip/info.php）等的时候，有可能是apache没有启动服务！

  

* * *

目前手机不能通过4G网连接zdkit.ticp.io,但是可以用ip访问到首页，通过ip的话智能进入首页，看到网站，不能点击子网页

但是可以通过wifi正常访问zdkit.ticp.io和ip地址！正常操作！

原因为dns的主机记录填错，应该是www，但是我填成了zdkit.ticp.io

  

* * *

  

在functions.php文件添加以下代码即可去掉后台的wordpress的logo

function \_admin\_bar_remove() {  
        global $wp\_admin\_bar;  
        $wp\_admin\_bar->remove_menu('wp-logo');  
}

add\_action('wp\_before\_admin\_bar\_render', '\_admin\_bar\_remove', 0);

  

* * *

关闭SELinux

SELinux是一个由美国国家安全局和SCC开发的 Linux的一个扩张强制访问控制安全模块。它可以保护Linux，但是开着SELinux有时候会发生一些莫名其妙的问题。所以在这里还是关掉算了。

先修改一下配置文件

  

vim /etc/selinux/config

在Vim编辑器中找到

#SELINUX=enforcing （在前面加#注释掉）

#SELINUXTYPE=targeted （在前面加#注释掉）

SELINUX=disabled （增加此行）

  

修改结束后保存退出

修改配置文件之后并不能立即关闭SELinux，我们还需要键入以下命令来立即关闭SELinux：

  

setenforce 0

  

* * *

把当前安装的主题文件夹重命名的话，会强制 WordPress 自动选择默认的主题，然后就可以正常载入了。

  
  

[来自为知笔记(Wiz)](http://www.wiz.cn/i/2381a414 "来自为知笔记(Wiz)")