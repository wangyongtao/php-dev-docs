Lumen / Laravel 5.4 使用网易邮箱 SMTP 发送邮件


## 获取网易邮箱的服务器和授权码:

登录网易邮箱 (http://mail.163.com/),

* 1. 获取服务器地址：  
点击【设置】 > 【POP3/SMTP/IMAP】: 

服务器地址:
```
    POP3服务器: pop.163.com
    SMTP服务器: smtp.163.com
    IMAP服务器: imap.163.com
```

* 2. 获取客户端授权密码  

> 授权码  
> 授权码是用于登录第三方邮件客户端的专用密码。   
> 适用于登录以下服务: POP3/IMAP/SMTP/Exchange/CardDAV/CalDAV服务。    

点击【设置】 > 【客户端授权密码】  
点击【开启】， 设置一个授权码， 比如本例中将授权码设置为: `mailPASSWORD`


## 配置 env 文件:  

在配置文件 `.env`文件，新增以下配置: 

```
MAIL_DRIVER=smtp  
MAIL_HOST=smtp.163.com
MAIL_PORT=25
MAIL_USERNAME=cnwytnet@163.com
MAIL_PASSWORD=mailPASSWORD
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS=cnwytnet@163.com
MAIL_FROM_NAME=cnwytnet
```

## Lumen 项目 

由于 Lumen 是简化版的 Laravel, 需要增加以下发邮件的模块。

* 需要添加 `illuminate/mail` 模块: 

修改`composer.json` 文件中 require 部分配置如下: 

```
    "require": {
        "php": ">=5.6.9",
        "laravel/lumen-framework": "5.4.*",
        "vlucas/phpdotenv": "~2.2",
        "guzzlehttp/guzzle": "^6.2",
        "predis/predis": "^1.1",
        "illuminate/redis": "^5.4",
        "illuminate/mail":"5.4.*"
    }
```

执行 `composer up`.

* 需要增加mail.php配置文件:

确保Luemn项目中存在 `app/config/mail.php` 配置文件。  
若不存在可以从 Laravel 代码中复制一份。


## 创建发邮件脚本  

* 创建脚本文件 `app/Console/Command/SendMailCommand.php`

```
<?php
namespace App\Console\Commands;

use Illuminate\Console\Command;
use Illuminate\Support\Facades\Mail;

class SendMailCommand extends Command
{
    /**
    * The name and signature of the console command.
    *
    * @var string
    */
    protected $signature = 'demo:SendMail';

    /**
    * The console command description.
    *
    * @var string
    */
    protected $description = '命令行-测试脚本-SendMail';

    /**
     * constructor
     */
    public function __construct()
    {
        parent::__construct();
    }

    /**
     * Execute the console command.
     *
     * @return mixed
     */
    public function handle()
    {
        $content = '这是一封来自Laravel的测试邮件.';
        $toMail  = 'wangtom365@qq.com';
        
        Mail::raw($content, function ($message) use ($toMail) {
            $message->subject('[ 测试 ] 测试邮件SendMail - ' .date('Y-m-d H:i:s'));
            $message->to($toMail);
        });
    }
}
```

* 将脚本文件加入到 app/Console/Kernel.php 中: 

```
    protected $commands = [
        Commands\SendMailCommand::class, //测试发邮件脚本
    ];
```

## 执行发邮件操作  

* 查看脚本， 可以看到我们新加的脚本命令 `demo:SendMail`:

```
$ php artisan 
demo
  demo:SendMail    命令行-测试脚本-SendMail
```

* 执行发送邮件脚本: 

```
$ php artisan demo:SendMail
```

不出意外的话，邮件发送成功。查看发件人的发件箱，或者查看收件人的收件箱，确认一下吧。


## 其他   

* 邮件地址 `MAIL_FROM_ADDRESS` 必须和 `MAIL_USERNAME`一致，否则报错:

```
    [Swift_TransportException]                                                                             
    Expected response code 250 but got code "553", with message "553 Mail from must equal authorized user" 
```

* 不填授权码 `MAIL_PASSWORD` 或者 `MAIL_PASSWORD` 错误，报错:

```
    [Swift_TransportException]                                                                               
    Failed to authenticate on SMTP server with username "cnwytnet@163.com" using 2 possible authenticators 
```

可以将邮件驱动改成 `MAIL_DRIVER=log`, 就可以在本地日志中看到邮件内容了，这在测试的时候会很有用。

比如，在配置`.env`中，修改邮件驱动为`MAIL_DRIVER=log`，将会把邮件发送内容保存到 `storage/logs/laravel.log` 中。
内容如下:

```
[2017-04-01 06:12:19] local.DEBUG: Message-ID: <727877e080177bbb349b98a869f5b20f@swift.generated>
Date: Sat, 01 Apr 2017 06:12:19 +0000
Subject: [ =?utf-8?Q?=E6=B5=8B=E8=AF=95?= ] SendMail - 2017-04-01 06:12:19
From: SendMailTEST <cnwytnet@163.com>
To: wangtom365@qq.com
MIME-Version: 1.0
Content-Type: text/plain; charset=utf-8
Content-Transfer-Encoding: quoted-printable

这是一封来自Laravel的测试邮件.  

```


END.

## 参考链接: 

https://laravel.com/docs/5.4/mail  

http://laravelacademy.org/post/1986.html  

