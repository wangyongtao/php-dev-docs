
安装php和php-fpm
这里我使用的php7版本，目前官方已经发布了php7的beta3版本。
php-fpm已经被包含在里面了，无需另外安装.

安装nginx

#启动php-fpm，mac必须要以root用户启动，-R 参数表示 --allow-to-run-as-root
sudo /usr/local/php7/sbin/php-fpm -R


sudo /data/webserver/php7/sbin/php-fpm -R


sudo /usr/local/webserver/nginx/nginx -t  

重新加载：
$ sudo /usr/local/webserver/nginx/nginx -s reload


http://localhost/phpinfo.php

/data/webserver/php7/etc/php-fpm.d/www-data.conf
指定了监听端口：

listen = 127.0.0.1:9000


/usr/local/webserver/nginx/nginx.conf
``` 
location ~ \.php$ {
    root           /usr/local/webserver/nginx/html;
    fastcgi_pass   127.0.0.1:9000;
    fastcgi_index  index.php;
    fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    include        fastcgi_params;
}
``` 

## 参考链接：

https://segmentfault.com/a/1190000003067656