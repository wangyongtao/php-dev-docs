php7-install.md

源码安装PHP7:

$ cat /System/Library/CoreServices/SystemVersion.plist
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>ProductBuildVersion</key>
	<string>14D136</string>
	<key>ProductCopyright</key>
	<string>1983-2015 Apple Inc.</string>
	<key>ProductName</key>
	<string>Mac OS X</string>
	<key>ProductUserVisibleVersion</key>
	<string>10.10.3</string>
	<key>ProductVersion</key>
	<string>10.10.3</string>
</dict>
</plist>

php7下载地址：https://downloads.php.net/~ab/



$ wget https://downloads.php.net/~ab/php-7.0.0alpha1.tar.bz2

$ tar jxf php-7.0.0alpha1.tar.bz2


./configure --prefix=/usr/local/php7 --with-config-file-path=/usr/local/php7/etc --enable-fpm --with-fpm-user=www--with-fpm-group=www --with-mysqli --with-pdo-mysql --with-iconv-dir --with-freetype-dir --with-jpeg-dir --with-png-dir --with-zlib --with-libxml-dir=/usr --enable-xml --disable-rpath --enable-bcmath --enable-shmop --enable-sysvsem--enable-inline-optimization --with-curl --enable-mbregex --enable-mbstring--with-mcrypt --enable-ftp --with-gd --enable-gd-native-ttf --with-openssl--with-mhash --enable-pcntl --enable-sockets --with-xmlrpc --enable-zip--enable-soap --without-pear --with-gettext --disable-fileinfo--enable-maintainer-zts


./configure \
--prefix=/usr/local/php7 \
--with-config-file-path=/usr/local/php7/etc \
--with-iconv=/usr/local/lib/libiconv \


