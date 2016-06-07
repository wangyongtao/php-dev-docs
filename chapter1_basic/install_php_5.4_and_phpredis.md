install_php_5.4.md

安装php-5.4.44环境

由于某些原因，需要安装php5.4以及phpredis扩展，现记录如下。


从PHP 5.4.0起， CLI SAPI 提供了一个内置的Web服务器。

> 说明:  
> 安装目录： /usr/local/webserver/    
> 源码文件：  
> - php-5.4.44.tar.bz2    
> - phpredis-2.2.7.tar.gz  
> 主要操作: 安装PHP, 配置FPM与NGINX, 安装phpredis扩展

## 环境准备:

1 操作系统信息:  

```
$ sw_vers
ProductName:	Mac OS X
ProductVersion:	10.11.3
BuildVersion:	15D21
$ uname -mnprs
Darwin MacWangYongTao.local 15.3.0 x86_64 i386
```

2 确保已经安装好了nginx，本机已安装了nginx/1.8.1:  

```
$ /usr/local/webserver/nginx/nginx -v
nginx version: nginx/1.8.1
``` 

## 安装PHP5.4 

网址: http://cn2.php.net/downloads.php  
下载: php-5.4.44.tar.bz2

安顺序执行以下命令：

```
$ tar jxf php-5.4.44.tar.bz2
$ cd php-5.4.44
$ ./configure \
    --prefix=/usr/local/webserver/php54 \
    --with-config-file-path=/usr/local/webserver/php54 \
    --with-mysql \
    --with-mysqli \
    --enable-pdo \
    --with-pdo-mysql \
    --with-mysql-sock=/usr/local/mysql/data/MacWang.local.pid \
    --enable-cgi \
    --enable-fpm \
    --enable-sockets \
    --enable-mbstring \
    --enable-mbregex \
    --enable-bcmath \
    --enable-xml \
    --enable-zip \
    --with-zlib=/usr/local/lib/zlib \
    --with-gd \
    --with-png-dir=/usr/local/lib/libpng \
    --with-jpeg-dir=/usr/local/lib/libjpeg \
    --with-openssl=/usr/local/opt/openssl \
    --with-curl=/usr/local/curl \
    --with-mhash=/usr/local/lib/libmhash \
    --with-mcrypt=/usr/local/lib/libmcrypt \
$ make  
$ make test  
$ sudo make install  
```

以下是安装成功后的一些信息:

```
// 代码实例：  
WangTom$ sudo make install
Password:
Installing PHP CLI binary:        /usr/local/webserver/php54/bin/
Installing PHP CLI man page:      /usr/local/webserver/php54/php/man/man1/
Installing PHP FPM binary:        /usr/local/webserver/php54/sbin/
Installing PHP FPM config:        /usr/local/webserver/php54/etc/
Installing PHP FPM man page:      /usr/local/webserver/php54/php/man/man8/
Installing PHP FPM status page:   /usr/local/webserver/php54/php/fpm/
Installing PHP CGI binary:        /usr/local/webserver/php54/bin/
Installing PHP CGI man page:      /usr/local/webserver/php54/php/man/man1/
Installing build environment:     /usr/local/webserver/php54/lib/php/build/
Installing header files:          /usr/local/webserver/php54/include/php/
Installing helper programs:       /usr/local/webserver/php54/bin/
  program: phpize
  program: php-config
Installing man pages:             /usr/local/webserver/php54/php/man/man1/
  page: phpize.1
  page: php-config.1
Installing PEAR environment:      /usr/local/webserver/php54/lib/php/
[PEAR] Archive_Tar    - installed: 1.3.12
[PEAR] Console_Getopt - installed: 1.3.1
[PEAR] Structures_Graph- installed: 1.0.4
[PEAR] XML_Util       - installed: 1.2.3
[PEAR] PEAR           - installed: 1.9.5
Wrote PEAR system config file at: /usr/local/webserver/php54/etc/pear.conf
You may want to add: /usr/local/webserver/php54/lib/php to your php.ini include_path
/data/softwares/php-5.4.44/build/shtool install -c ext/phar/phar.phar /usr/local/webserver/php54/bin
ln -s -f /usr/local/webserver/php54/bin/phar.phar /usr/local/webserver/php54/bin/phar
Installing PDO headers:          /usr/local/webserver/php54/include/php/ext/pdo/

$ sudo /data/softwares/php-5.4.44/build/shtool install -c ext/phar/phar.phar /usr/local/webserver/php54/bin
$ sudo ln -s -f /usr/local/webserver/php54/bin/phar.phar /usr/local/webserver/php54/bin/phar
```

通过命令行，查看版本:  
```
$ /usr/local/webserver/php54/bin/php -v  
PHP 5.4.44 (cli) (built: May 18 2016 19:17:07)   
```

注意事项:

--enable-fastcgi(已废弃)
如果启用，CGI 模块将被编译为支持 FastCGI。PHP 4.3.0 之后的版本有效。
PHP 5.3.0起，此参数不再存在，并使用 --enable-cgi替代。

--enable-opcache(此选项在php5.5及以后版本自带，在php5.4中使用就会报unrecognized options)
configure: WARNING: unrecognized options: --enable-opcache  

--with-mysql-sock 如果没有安装mysql,这个选项可以先不加上. 
执行“ps -ef |grep mysql”命令，可以看到msyql pid的路径：“pid-file=/usr/local/mysql/data/MacWang.local.pid”  


## 配置FPM: 

FastCGI进程管理器(FPM）,编译 PHP 时需要 --enable-fpm 配置选项来激活 FPM 支持。此选项在本次在编译时已经加上了。

进入php54/etc/目录，复制一份配置:php-fpm.conf  
$ cd /usr/local/webserver/php54/etc/   
$ sudo cp php-fpm.conf.default php-fpm.conf  

测试FPM的配置文件是否正确  
$ sudo /usr/local/webserver/php54/sbin/php-fpm -t  

启动 php-fpm:  
$ sudo /usr/local/webserver/php54/sbin/php-fpm


操作实例: 
```
//可以使用参数-t测试FPM的配置(没有复制配置文件php-fpm.conf):
$ /usr/local/webserver/php54/sbin/php-fpm -t
[19-May-2016 10:38:42] ERROR: failed to open configuration file '/usr/local/php/etc/php-fpm.conf': No such file or directory (2)
[19-May-2016 10:38:42] ERROR: failed to load configuration file '/usr/local/php/etc/php-fpm.conf'
[19-May-2016 10:38:42] ERROR: FPM initialization failed

//可以使用参数-y指定本次安装的php54的路径
$ /usr/local/webserver/php54/sbin/php-fpm -t -y /usr/local/webserver/php54/etc/php-fpm.conf
[19-May-2016 10:40:55] ERROR: failed to open error_log (/usr/local/php/var/log/php-fpm.log): Permission denied (13)
[19-May-2016 10:40:55] ERROR: failed to post process the configuration
[19-May-2016 10:40:55] ERROR: FPM initialization failed

// 如果没有权限，请使用 sudo 或者给php-fpm.log权限:
$ /usr/local/webserver/php54/sbin/php-fpm -t
[20-May-2016 10:29:08] ERROR: failed to open error_log (/usr/local/webserver/php54/var/log/php-fpm.log): Permission denied (13)
[20-May-2016 10:29:08] ERROR: failed to post process the configuration
[20-May-2016 10:29:08] ERROR: FPM initialization failed
$ sudo /usr/local/webserver/php54/sbin/php-fpm -t  
[20-May-2016 10:32:33] NOTICE: configuration file /usr/local/webserver/php54/etc/php-fpm.conf test is successful  

// 如果php-fpm.log文件不存在，可以进入日志目录，创建一个日志文件，并给权限
$ cd /usr/local/webserver/php54/var/log  
$ sudo touch php-fpm.log  
$ sudo chmod 777 php-fpm.log  

//可以修改FPM配置文件中的日志路径及监听端口：
$ sudo vi /usr/local/webserver/php54/etc/php-fpm.conf  

> 修改FPM配置文件：(注释使用英文分号)
> 配置文件路径: /usr/local/webserver/php54/etc/php-fpm.conf  
> //20160519  
> ;//error_log = log/php-fpm.log (默认的地址)
>  error_log = /usr/local/webserver/php54/var/log/php-fpm.log  
>  
> // 比如可以将9000端口改成9001： 
> ;listen = 127.0.0.1:9000  
> listen = 127.0.0.1:9001  
> 
> 注意： 如果此处修改了FPM端口号，Nginx也要相应的修改:
> Nginx配置文件: /usr/local/webserver/nginx/nginc.conf
> fastcgi_pass   127.0.0.1:9001;

//出现的错误：报监听端口(127.0.0.1:9000)已经存在，是刚刚启动的，直接kill-9掉
$ sudo /usr/local/webserver/php54/sbin/php-fpm -y /usr/local/webserver/php54/etc/php-fpm.conf -R
[19-May-2016 10:56:03] ERROR: unable to bind listening socket for address '127.0.0.1:9000': Address already in use (48)
[19-May-2016 10:56:03] ERROR: FPM initialization failed  
$ ps -ef |grep fpm  
$ kill -9 11668 (这pid根据实际情况定)  

//重新启动，没有报错，OK
$ sudo /usr/local/webserver/php54/sbin/php-fpm -y /usr/local/webserver/php54/etc/php-fpm.conf -R

```

查看版本
```
$ /usr/local/webserver/php54/sbin/php-fpm -v
PHP 5.4.44 (fpm-fcgi) (built: May 18 2016 19:17:13)
```

安装完成后，如果发现/usr/local/webserver/php54/下没有PHP.ini配置文件，  
就需要手动从源码中负责一份:
$ locate php.ini-development
$ sudo cp /data/softwares/php-5.4.44/php.ini-development /usr/local/webserver/php54/php.ini

> 在phpinfo()页面中的部分信息：
> Server API:	FPM/FastCGI
> Virtual Directory Support:	disabled
> Configuration File (php.ini) Path:	/usr/local/webserver/php54
> Loaded Configuration File:	(none)
> Scan this dir for additional .ini files:	(none)
> Additional .ini files parsed:	(none)

看到 Loaded Configuration File 这一栏，信息是(none)

尝试修改了一个参数，重启Nginx，发现没有变化，肯定是php.ini没有生效。

但是，使用命令行，可以看到 Loaded Configuration File 的路径: 

```
$ /usr/local/webserver/php54/bin/php -i | grep "php.ini"
Configuration File (php.ini) Path => /usr/local/webserver/php54
Loaded Configuration File => /usr/local/webserver/php54/php.ini

$ /usr/local/webserver/php54/sbin/php-fpm -i |grep "php.ini"
Configuration File (php.ini) Path => /usr/local/webserver/php54
Loaded Configuration File => /usr/local/webserver/php54/php.ini
```

直接kill掉fpm进程，重新启动php-fpm，终于生效了:

可以看到：  
Loaded Configuration File	/usr/local/webserver/php54/php.ini

错误：ERROR: [pool wwwuser] cannot get uid for user 'wwwuser'
描述：修改php-fpm.conf，将配置文件中的 user 和 group 部分的 nobody 改成 www
$ sudo /usr/local/webserver/php54/sbin/php-fpm 
[23-May-2016 16:09:59] ERROR: [pool wwwuser] cannot get uid for user 'wwwuser'
[23-May-2016 16:09:59] ERROR: FPM initialization failed
原因：此用户名不能随便改，需要改成系统存在的用户或者新建的用户，比如"www"
解决：修改php-fpm.conf，将配置文件中的 user 和 group 的内容，从"wwwuser"再改成"www"

错误: fopen(43): failed to open stream: Permission denied
描述： 在代码中打开文件，写入日志，报错""
给目录777权限都不可以
var_dump(is_writable($filepath)); //输出false

echo getcwd(); //输出的是 "nobody"
修改php-fpm.conf配置文件，将配置文件中的 user 和 group 部分的 nobody 改成 www:
echo getcwd(); //输出的是 "_www"


错误新：ERROR: [pool wwwroot] cannot get uid for user 'wwwroot'
$ sudo /usr/local/webserver/php54/sbin/php-fpm 
[23-May-2016 16:09:59] ERROR: [pool wwwroot] cannot get uid for user 'wwwroot'
[23-May-2016 16:09:59] ERROR: FPM initialization failed
原因：此用户名不能随便改，需要改成系统存在的用户或者新建的用户，比如"www"

Mac下查看所有用户和组
dscacheutil -q group
$ cat /etc/group


## Nginx配置

修改nginx配置文件,使其能和php相互工作。

/usr/local/webserver/nginx/nginx.conf

``` 
//在nginx.conf中server{}中，增加以下代码:  
location ~ \.php$ {
    root           /usr/local/webserver/nginx/html; 
    fastcgi_pass   127.0.0.1:9001;
    fastcgi_index  index.php;
    fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    include        fastcgi_params;
}
``` 


通过浏览器，查看phpinfo信息：  

在nginx/html/目录下，放置了一个phpinfo.php文件，内容如下:

> 文件phpinfo.php中的内容:
> date_default_timezone_set("PRC");  
> phpinfo();  

```
$ pwd
/usr/local/webserver/nginx/html
$ cat phpinfo.php 
< ? php
date_default_timezone_set("PRC");
phpinfo();
```

打开浏览器，输入 http://localhost/phpinfo.php，即可看到相关新。


问题描述: 无论是在phpinfo.php页面中，还是命令行中，都可能会看到这个警告:  
```
PHP Warning:  
Unknown: It is not safe to rely on the system's timezone settings. 
You are *required* to use the date.timezone setting or the date_default_timezone_set() function. 
In case you used any of those methods and you are still getting this warning, you most likely misspelled the timezone identifier.
We selected the timezone 'UTC' for now, but please set date.timezone to select your timezone. in Unknown on line 0.
```

问题分析:
 这是PHP5.4的新特性：
 必须使用 date.timezone php.ini 配置选项或 date_default_timezone_set() 函数来指定时区。
 PHP 将不再尝试猜测时区，而是回退到“UTC”并发出一条 E_WARNING 错误。

处理办法： 
 在php.ini中配置时区即可: 
 > 打开date.timezone前的分号注释，写上时区，比如"Asia/Shanghai":  
 > date.timezone = Asia/Shanghai


## 安装PhpRedis扩展


PhpRedis, 是用C语言编写的PHP模块，用来连接并操作Redis数据库。

The phpredis extension provides an API for communicating with the Redis key-value store. 

网址: https://github.com/phpredis/phpredis  
代码:https://github.com/phpredis/phpredis/releases  
本次下载的版本: phpredis-2.2.7.tar.gz  


$ tar zxvf phpredis-2.2.7.tar.gz   
$ cd phpredis-2.2.7    
$ /usr/local/webserver/php54/bin/phpize  

```
//遇到错误，找不到autoconf，使用brew安装一下: 
$ /usr/local/webserver/php54/bin/phpize
Configuring for:
PHP Api Version:         20100412
Zend Module Api No:      20100525
Zend Extension Api No:   220100525
Cannot find autoconf. Please check your autoconf installation and the
$PHP_AUTOCONF environment variable. Then, rerun this script.

//使用brew安装autoconf: 
$ brew install autoconf
```


```
$ /usr/local/webserver/php54/bin/phpize
Configuring for:
PHP Api Version:         20100412
Zend Module Api No:      20100525
Zend Extension Api No:   220100525
```

指定php配置文件路径

$ ./configure --with-php-config=//usr/local/webserver/php54//bin/php-config
$ make  
$ make test
$ sudo make install 


```
//安装成功后，出现扩展的路径:   
$ sudo make install
Password:
Installing shared extensions:/usr/local/webserver/php54/lib/php/extensions/no-debug-non-zts-20100525/
```

注意redis.so扩展的路径：

/usr/local/webserver/php54/lib/php/extensions/no-debug-non-zts-20100525/


修改php.ini配置文件，指定redis.so文件路径:

> 如果不写绝对地址，需要做个软连接，否则可能会出错:  
> extension=redis.so
> 或者：
> extension=/usr/local/webserver/php54/lib/php/extensions/no-debug-non-zts-20100525/redis.so

注意：如果不想写全路径，可以执行以下命令，做个软连接:  

ln -s /usr/local/webserver/php54/lib/php/extensions/no-debug-non-zts-20100525/redis.so redis.so  

浏览器输入: http://localhost/phpinfo.php
即可查看到已经安装了redis扩展了。


查看已安装扩展(命令行):

```
// 可以使用 -m 参数来列出所有有效的扩展
$ /usr/local/webserver/php54/bin/php -m |grep redis
redis
```


## 参考链接：

http://php.net/downloads.php  
https://github.com/phpredis/phpredis#classes-and-methods  
https://github.com/phpredis/phpredis/issues/468  
http://php.net/manual/zh/migration54.incompatible.php  


## 更新记录： 

2016-05-18: 新增安装php5.4.44环境
2016-05-19: 新增安装phpredis扩展


[END]



