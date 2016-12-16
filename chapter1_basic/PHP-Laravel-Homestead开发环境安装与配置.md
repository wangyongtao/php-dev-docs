# PHP Laravel-Homestead 开发环境安装与配置

> Laravel Homestead 是一个官方预载的 Vagrant「封装包」，提供你一个美好的开发环境，不需要在本机端安装 PHP、HHVM、网页服务器或任何服务器软件。
> Homestead 可以在任何 Windows、Mac 或 Linux 上面运行，里面包含了 Nginx 网页服务器、PHP 5.6、MySQL、Postgres、Redis、Memcached等软件，
> 还有所有你要开发精彩的 Laravel 应用程序所需的软件。

### 本机系统环境:

```
//使用 uname 查看当前操作系统名称
$ uname -mnprs
Darwin YONG-TEST.local 15.6.0 x86_64 i386

//使用sw_vers 查看 macOS版本信息
$ sw_vers
ProductName:  Mac OS X
ProductVersion: 10.11.6
BuildVersion: 15G31
``

### 不使用用Homestead，直接在本机安装:

使用homebrew安装PHP7:

$ brew update
$ brew search php70
homebrew/php/php70               
homebrew/php/php70-gmagick       
homebrew/php/php70-maxminddb     
homebrew/php/php70-pdo-pgsql     
homebrew/php/php70-stats       
homebrew/php/php70-amqp
...

$ brew install homebrew/php/php70

配置php-fpm端口为9002， 因为本机已经安装了php5, 占用了9001端口。

配置Nginx配置文件：

重启Nginx服务器。


### 安装Homestead Vagrant Box:

下载 Homestead:

```
$ git clone https://github.com/laravel/homestead.git Homestead
$ cd Homestead/
$ ls
CHANGELOG.md Vagrantfile composer.lock init.bat readme.md /src
LICENSE.txt composer.json homestead init.sh /scripts
$ bash init.sh
```

下载 Homestead Vagrant Box：

$ vagrant box add laravel/homestead

执行示例:

```
//下载安装laravel/homestead：
$ vagrant box add laravel/homestead --force
==> box: Loading metadata for box 'laravel/homestead'
box: URL: https://atlas.hashicorp.com/laravel/homestead
==> box: Adding box 'laravel/homestead' (v0.5.0) for provider: virtualbox
box: Downloading: https://atlas.hashicorp.com/laravel/boxes/homestead/versions/0.5.0/providers/virtualbox.box
box: Progress: 5% (Rate: 572k/s, Estimated time remaining: 0:33:13)
...

//下载成功后显示如下:
==> box: Successfully added box 'laravel/homestead' (v0.5.0) for 'virtualbox'!
```

### 启动虚拟机

```
$ vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
There are errors in the configuration of this machine. Please fix the following errors and try again:
vm: * The host path of the shared folder is missing: ~/Code
//报错没有“~/Code”目录，那我们就创建一个:
$ mkdir ~/Code
$ vagrant up
```

打开VirtualBox中，可以看到有一个名为"homestead-7"的虚拟机正在运行(状态为:Running)。


### 配置Hosts文件:

```
$ sudo vi /etc/hosts
//增加一条记录:  
192.168.10.10  homestead.app
```

Make sure the IP address listed is the one set in your ~/.homestead/Homestead.yaml file.


### 使用laravel创建一个站点:  

$ cd ~/Code
$ composer require laravel/homestead --dev

配置文件“Homestead.yaml”里包含一个示例站点配置.
注意默认的站点是"/home/vagrant/Code/Laravel/public",
所以我们现在创建在~/Code下面创建一个名为"Laravel"。

$ laravel new Laravel
-bash: laravel: command not found

$ ~/.composer/vendor/bin/laravel new Laravel

/home/vagrant/Code/Laravel/public

打开浏览器，输入"http://homestead.app/", 即可正常访问。


### 配置 Homestead

配置文件路径是: ~/.homestead/Homestead.yaml

配置文件内容如下:
```
ip: "192.168.10.10"
memory: 2048
cpus: 1
provider: virtualbox
authorize: ~/.ssh/id_rsa.pub
keys:
    - ~/.ssh/id_rsa
folders:
    - map: ~/Code
      to: /home/vagrant/Code
sites:
    - map: homestead.app
      to: /home/vagrant/Code/Laravel/public
databases:
    - homestead
```

配置示例: 新增一个网站"kaixin123.app":

(1) 新增网站目录"kaixin123net":
  在Code目录中新增一个kaixin123net目录，作为新增网站的根目录:
  创建一个index.html文件，随便写写内容，比如"hello,kaixin123.app":
  路径如下: ~/Code/kaixin123net/index.html

(2) 增加sites：
```
$ vim ~/.homestead/Homestead.yaml

//在sites中新增一条map记录,修改后配置如下:
//配置中的xunij  “/home/vagrant/Code/kaixin123net” 对应我们本地的 "~/Code/kaixin123net"
sites:
    ### 默认
    - map: homestead.app
      to: /home/vagrant/Code/Laravel/public
    ### 新增加一个网站，域名为kaixin123.app
    - map: kaixin123.app
      to: /home/vagrant/Code/kaixin123net
```
(3) 新增一条host记录:
```
$ sudo vim /etc/hosts

//新增一条host记录，IP是Homestead.yaml中“ip”，修改后如下：
... (略) ...
192.168.10.10 homestead.app
192.168.10.10 kaixin123.app

```
(4) 打开浏览器，输入"http://kaixin123.app/", 可以正常访问。


登录虚拟机:

$ cd ~/Homestead/  
$ vagrant ssh  

```
/进入~/Homestead/目录
YONGTEST:~ WangTom$ cd ~/Homestead/
YONGTEST:Homestead WangTom$

//登录虚拟机
YONGTEST:Homestead WangTom$ vagrant ssh
Welcome to Ubuntu 16.04 LTS (GNU/Linux 4.4.0-22-generic x86_64)
 * Documentation:  https://help.ubuntu.com/
Last login: Fri Sep 23 06:42:33 2016 from 10.0.2.2

//有Code文件夹，这个/home/vagrant/Code与本机的~/Code目录会保持同步
vagrant@homestead:~$ ls
Code

//退出虚拟机
vagrant@homestead:~$ exit
logout
Connection to 127.0.0.1 closed.
```


连接Homestead数据库:

>
> To connect to your MySQL or Postgres database from your host machine via Navicat or Sequel Pro:
> Connect to 127.0.0.1 and port 33060 (MySQL) or 54320 (Postgres).
> The username and password for both databases is homestead / secret.

地址: 192.168.10.10  
端口: 3306   
用户名: homestead   
密码: secret  


登录虚拟机数据库:

```
vagrant@homestead:~$ mysql -h192.168.10.10 -uhomestead -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 51
Server version: 5.7.13-0ubuntu0.16.04.2 (Ubuntu)
Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.
Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| homestead          |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.00 sec)
mysql>
```

以后再次启动/登录:
$ cd ~/Homestead/  
$ vagrant up  
$ vagrant ssh


### 扩展阅读

(1) brew  

brew(即Homebrew), 是macOS上的软件包管理工具, 能在Mac中方便的安装软件或者卸载软件,类似ubuntu系统下的apt-get的功能.
Homebrew- The missing package manager for OS X
http://brew.sh/

(2) Vagrant  

Vagrant是一个基于Ruby的工具，用于创建和部署虚拟化开发环境。它使用Oracle的开源VirtualBox虚拟化系统，使用Chef创建自动化虚拟环境。
https://www.vagrantup.com/

(3) Laravel  

Laravel是一套简洁、优雅的PHP Web开发框架(PHP Web Framework)。
https://laravel.com/


### 参考资料

https://laravel.com/docs/5.3/homestead   
http://laravelacademy.org/post/2749.html   
http://laravel-china.org/docs/5.1/homestead   
https://www.vagrantup.com/docs/getting-started/   


### 更新记录

1. 20160825 新增此文档 (wangyt)
2. 20160829 更新文档，改成Homestead安装与配置 (wangyt)
3. 20160922 更新文档，新增配置多站点 (wangyt)
