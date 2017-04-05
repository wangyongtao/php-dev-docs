安装php-5.5环境

由于某些原因，需要安装php5.4以及phpredis扩展，现记录如下。

$ cd ~/Homestead
$ vagrant ssh

进入vagrant 虚拟机中

创建`/usr/local/webserver/` 目录: 

下载地址: http://cn2.php.net/releases/
选择: php-5.5.38 


$ sudo ./configure \
    --prefix=/usr/local/webserver/php55 \
    --with-config-file-path=/usr/local/webserver/php55 \
    --with-mysql \
    --with-mysqli \
    --enable-pdo \
    --with-pdo-mysql \
    --enable-cgi \
    --enable-fpm \
    --enable-sockets \
    --enable-mbstring \
    --enable-mbregex \
    --enable-bcmath \
    --enable-xml \
    --enable-zip \
    --with-gd

$ sudo make

vagrant@homestead:/usr/local/webserver/php-5.5.38$ sudo make install
Installing shared extensions:     /usr/local/webserver/php55/lib/php/extensions/no-debug-non-zts-20121212/
Installing PHP CLI binary:        /usr/local/webserver/php55/bin/
Installing PHP CLI man page:      /usr/local/webserver/php55/php/man/man1/
Installing PHP FPM binary:        /usr/local/webserver/php55/sbin/
Installing PHP FPM config:        /usr/local/webserver/php55/etc/
Installing PHP FPM man page:      /usr/local/webserver/php55/php/man/man8/
Installing PHP FPM status page:      /usr/local/webserver/php55/php/php/fpm/
Installing PHP CGI binary:        /usr/local/webserver/php55/bin/
Installing PHP CGI man page:      /usr/local/webserver/php55/php/man/man1/
Installing build environment:     /usr/local/webserver/php55/lib/php/build/
Installing header files:          /usr/local/webserver/php55/include/php/
Installing helper programs:       /usr/local/webserver/php55/bin/
  program: phpize
  program: php-config
Installing man pages:             /usr/local/webserver/php55/php/man/man1/
  page: phpize.1
  page: php-config.1
Installing PEAR environment:      /usr/local/webserver/php55/lib/php/
[PEAR] Archive_Tar    - installed: 1.4.0
[PEAR] Console_Getopt - installed: 1.4.1
[PEAR] Structures_Graph- installed: 1.1.1
[PEAR] XML_Util       - installed: 1.3.0
[PEAR] PEAR           - installed: 1.10.1
Wrote PEAR system config file at: /usr/local/webserver/php55/etc/pear.conf
You may want to add: /usr/local/webserver/php55/lib/php to your php.ini include_path
/usr/local/webserver/php-5.5.38/build/shtool install -c ext/phar/phar.phar /usr/local/webserver/php55/bin
ln -s -f phar.phar /usr/local/webserver/php55/bin/phar
Installing PDO headers:          /usr/local/webserver/php55/include/php/ext/pdo/


通过命令行，查看版本:  

```
$ /usr/local/webserver/php55/bin/php -v
PHP 5.5.38 (cli) (built: Apr  5 2017 08:25:03)
Copyright (c) 1997-2015 The PHP Group
Zend Engine v2.5.0, Copyright (c) 1998-2015 Zend Technologies
```

```
$ sudo ln -s /usr/local/webserver/php55/bin/php /usr/local/bin/php55

$ php55 --version  
PHP 5.5.38 (cli) (built: Apr  5 2017 08:25:03)
Copyright (c) 1997-2015 The PHP Group
Zend Engine v2.5.0, Copyright (c) 1998-2015 Zend Technologies

$ php --version
PHP 7.0.8-2+deb.sury.org~xenial+1 (cli) ( NTS )
Copyright (c) 1997-2016 The PHP Group
Zend Engine v3.0.0, Copyright (c) 1998-2016 Zend Technologies
```


## 配置FPM: 

FastCGI进程管理器(FPM）,编译 PHP 时需要 --enable-fpm 配置选项来激活 FPM 支持。此选项在本次在编译时已经加上了。

进入 `php55/etc/` 目录，复制一份配置`php-fpm.conf`:  

``` 
$ cd /usr/local/webserver/php55/etc/   
$ sudo cp php-fpm.conf.default php-fpm.conf  
``` 

测试FPM的配置文件是否正确  

```
$ sudo /usr/local/webserver/php55/sbin/php-fpm -t
[05-Apr-2017 08:52:37] NOTICE: configuration file /usr/local/webserver/php55/etc/php-fpm.conf test is successful
```

启动 `php-fpm`:  
$ sudo /usr/local/webserver/php55/sbin/php-fpm

$ sudo /usr/local/webserver/php55/sbin/php-fpm
[05-Apr-2017 08:54:05] ERROR: [pool www] cannot get gid for group 'nobody'
[05-Apr-2017 08:54:05] ERROR: FPM initialization failed


$ sudo groupadd nobody

$ sudo /usr/local/webserver/php55/sbin/php-fpm
[05-Apr-2017 08:55:03] ERROR: unable to bind listening socket for address '127.0.0.1:9000': Address already in use (98)
[05-Apr-2017 08:55:03] ERROR: FPM initialization failed


//可以修改FPM配置文件中的日志路径及监听端口：
$ sudo vi /usr/local/webserver/php55/etc/php-fpm.conf  


修改FPM配置文件：(注释使用英文分号)

> 配置文件路径: /usr/local/webserver/php54/etc/php-fpm.conf  
> ;//error_log = log/php-fpm.log (默认的地址)
>  error_log = /usr/local/webserver/php54/var/log/php-fpm.log  
>  
> // 比如可以将9000端口改成9001： 
> ;listen = 127.0.0.1:9000  
> listen = 127.0.0.1:9055  


创建 Nginx 配置文件:

sudo vi /etc/nginx/conf.d/adminlocal.boqii.com.conf

注意： 如果此处的FPM端口号要和 `php-fpm.conf` 中配置的相同:
 Nginx配置文件: /usr/local/webserver/nginx/nginc.conf
 `fastcgi_pass 127.0.0.1:9055;`


完整的配置如下

```
server {
    listen 80;
    server_name adminlocal.boqii.com;
    root "/home/vagrant/Code/admin";

    index index.html index.htm index.php;

    charset utf-8;

    location / {

        if ($request_method = 'OPTIONS') {
            add_header Access-Control-Allow-Origin *;
            add_header Access-Control-Allow-Methods GET,POST,PUT,DELETE,OPTIONS;
            add_header Access-Control-Allow-Headers Content-Type,app-id,signature,timestamp,nonce,Authorization;
            add_header Allow OPTIONS,HEAD,DELETE,POST,GET;
            return 204;
        }

        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    access_log off;
    error_log  /var/log/nginx/adminlocal.boqii.com-error.log error;

    sendfile off;

    client_max_body_size 100m;

    location ~ \.php$ {
        add_header Access-Control-Allow-Origin *;
        add_header Access-Control-Allow-Methods GET,POST,PUT,DELETE,OPTIONS;
        add_header Access-Control-Allow-Headers Content-Type,app-id,signature,timestamp.nonce,Authorization;
        add_header Allow OPTIONS,HEAD,DELETE,POST,GET;

        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass 127.0.0.1:9055;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;

        fastcgi_intercept_errors off;
        fastcgi_buffer_size 16k;
        fastcgi_buffers 4 16k;
        fastcgi_connect_timeout 300;
        fastcgi_send_timeout 300;
        fastcgi_read_timeout 300;
    }
}
```


## 安装PhpRedis扩展


PhpRedis, 是用C语言编写的PHP模块，用来连接并操作Redis数据库。

The phpredis extension provides an API for communicating with the Redis key-value store. 

网址: https://github.com/phpredis/phpredis  
代码: https://github.com/phpredis/phpredis/releases  
本次下载的版本: phpredis-2.2.7.tar.gz  
 
``` 
$ sudo wget https://github.com/phpredis/phpredis/archive/3.1.2.tar.gz
$ tar zxvf 3.1.2.tar.gz
$ cd phpredis-3.1.2
$ sudo /usr/local/webserver/php55/bin/phpize
 Configuring for:
 PHP Api Version:         20121113
 Zend Module Api No:      20121212
 Zend Extension Api No:   220121212

$ sudo ./configure --with-php-config=/usr/local/webserver/php55/bin/php-config
$ sudo make  
$ sudo make test
$ sudo make install
Installing shared extensions: /usr/local/webserver/php55/lib/php/extensions/no-debug-non-zts-20121212/

```

注意redis.so扩展的路径：
/usr/local/webserver/php55/lib/php/extensions/no-debug-non-zts-20121212/


修改php.ini配置文件，指定redis.so文件路径:

```
 extension=redis.so

```

如果在php55目录中，没有发现有 php.ini 文件，那就去 php 源码中复制一份:  

```
$ cd /usr/local/webserver/php55 

$ php55 -i | grep php.ini
Configuration File (php.ini) Path => /usr/local/webserver/php55

$ find /usr/local/webserver/ -name 'php.ini*'
/usr/local/webserver/php-5.5.38/php.ini-development
/usr/local/webserver/php-5.5.38/php.ini-production

$ sudo cp /usr/local/webserver/php-5.5.38/php.ini-development /usr/local/webserver/php55/php.ini
```

如果不写绝对地址，需要做个软连接，否则可能会出错:  

```
 extension=redis.so
 
 或者：
  extension=/usr/local/webserver/php55/lib/php/extensions/no-debug-non-zts-20100525/redis.so
```

注意：如果不想写全路径，可以执行以下命令，做个软连接:  

sudo ln -s ln -s /usr/local/webserver/php55/lib/php/extensions/no-debug-non-zts-20121212/redis.so redis.so  

 


查看已安装扩展(命令行):

```
// 可以使用 `-m` 参数来列出所有有效的扩展
$ /usr/local/webserver/php55/bin/php -m |grep redis
redis
```












