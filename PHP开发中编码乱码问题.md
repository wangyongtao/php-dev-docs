PHP乱码的一些问题

PHP开发中出现乱码，一般在文件、数据、各处的格式不同造成的。 

请包装PHP文件格式为UTF8，模板HTML/CSS/JavaScript等文件也要保存成UTF8格式。

操作：
Notepad++: 打开文件，选择[格式(M)]>[转为UTF8无BOM编码格式]，然后保存即可




在HTML页面中声明编码格式：
在页面< head >< /head >里面增加meta信息：
<meta charset="UTF-8">
<meta http-equiv="Content-Type" content="text/html;charset=UTF-8">

在PHP文件中使用UTF8编码：
header("Content-Type:text/html; charset=utf-8");

数据库使用UTF8:
创建数据库的时候，MySQL字符集选择'UTF8'，MySQL连接校对选择utf8_general_ci

$mysqli->query("SET character set 'utf8'");//读库 
$mysqli->query("SET NAMES 'utf8'");//写库 


#### 问题1：在Linux下执行PHP文件时错误：Could not open input file
解决：如果出现这个，可能就是文件格式的问题：
使用Vim命令：在Linux上查看文档格式 :set ff
	fileformat=dos 
执行 :set ff=unix
再次查看 :set ff
	fileformat=unix
变成了unix格式的，重试执行PHP文件，正常。

使用Notepad++编辑器也可以改变格式：
[编辑(E)] > [档案格式转换] > [转换为UNINX格式]


#### PHP中有关编码的函数

##### iconv — 字符串按要求的字符编码来转换
 格式：string iconv ( string $in_charset , string $out_charset , string $str )
 说明：将字符串 str 从 in_charset 转换编码到 out_charset。 

##### urlencode — 编码 URL 字符串
 格式：string urlencode ( string $str )
 说明：此函数便于将字符串编码并将其用于 URL 的请求部分，同时它还便于将变量传递给下一页。 

##### urldecode — 解码已编码的 URL 字符串
 格式：string urldecode ( string $str )
 说明：解码给出的已编码字符串中的任何 %##。 加号（'+'）被解码成一个空格字符。

##### rawurlencode() - 按照 RFC 1738 对 URL 进行编码
##### rawurldecode() - 对已编码的 URL 字符串进行解码

##### json_encode — 对变量进行 JSON 编码
##### json_decode — 对 JSON 格式的字符串进行编码

##### serialize — 产生一个可存储的值的表示 
##### unserialize — 从已存储的表示中创建 PHP 的值

##### session_encode — 将当前会话数据编码为一个字符串
##### session_decode() - Decodes session data from a session encoded string


