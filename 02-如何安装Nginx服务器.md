02-如何源码安装Nginx服务器

# 02-如何源码安装Nginx服务器

### 安装 PCRE :
> # 网站：http://pcre.org/   
> # 下载：
> - ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/
> - ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.36.tar.bz2

### 安装 Nginx :
> + 网站地址：http://nginx.org/en/docs/
> + 下载地址：http://nginx.org/download/nginx-1.8.0.tar.gz

安装命令:
```
下载：
wget http://nginx.org/download/nginx-1.8.0.tar.gz

tar zxf nginx-1.8.0.tar.gz
cd nginx-1.8.0
./configure --prefix=/usr/local/nginx
make
sudo make install

ps -ef |grep nginx

启动Nginx: 
sudo /usr/local/nginx/sbin/nginx
```
查看Nginx版本：
```
/usr/local/nginx/sbin/nginx -V
Mac-mini:nginx-1.8.0 WangTom$ /usr/local/nginx/sbin/nginx -V
nginx version: nginx/1.8.0
built by clang 6.1.0 (clang-602.0.53) (based on LLVM 3.6.0svn)
configure arguments: --prefix=/usr/local/nginx

Mac-mini:nginx-1.8.0 WangTom$ /usr/local/nginx/sbin/nginx -v
nginx version: nginx/1.8.0
```

访问： http://localhost/    
页面出现：
```
Welcome to nginx! 

If you see this page, the nginx web server is successfully installed and working. 
Further configuration is required.
For online documentation and support please refer to nginx.org.
Commercial support is available at nginx.com.Thank you for using nginx.
```

[END]