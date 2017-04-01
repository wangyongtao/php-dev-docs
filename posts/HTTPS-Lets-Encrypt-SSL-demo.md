HTTPS-Lets-Encrypt-SSL-demo.md


Certbot, previously the Let's Encrypt Client, is EFF's tool to obtain certs from Let's Encrypt, 
and (optionally) auto-enable HTTPS on your server. 
It can also act as a client for any other CA that uses the ACME protocol.


$ wget https://github.com/certbot/certbot/archive/v0.12.0.tar.gz

$ ./certbot-auto certonly
 
Saving debug log to /var/log/letsencrypt/letsencrypt.log

How would you like to authenticate with the ACME CA?
-------------------------------------------------------------------------------
1: Apache Web Server plugin - Beta (apache)
2: Place files in webroot directory (webroot)
3: Spin up a temporary webserver (standalone)
-------------------------------------------------------------------------------
Select the appropriate number [1-3] then [enter] (press 'c' to cancel): 2

worker_processes  1;
Please enter in your domain name(s) (comma and/or space separated)  (Enter 'c'
to cancel):wang123.net
Obtaining a new certificate
Performing the following challenges:
http-01 challenge for wang123.net

Select the webroot for wang123.net:
-------------------------------------------------------------------------------
1: Enter a new webroot
-------------------------------------------------------------------------------
Press 1 [enter] to confirm the selection (press 'c' to cancel): 1
Input the webroot for wang123.net: (Enter 'c' to cancel):/wwwdata/wwwroot/wang123net/public/
Waiting for verification...
Cleaning up challenges
Generating key (2048 bits): /etc/letsencrypt/keys/0000_key-certbot.pem
Creating CSR: /etc/letsencrypt/csr/0000_csr-certbot.pem

IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at
   /etc/letsencrypt/live/wang123.net/fullchain.pem. Your cert will
   expire on 2017-06-30. To obtain a new or tweaked version of this
   certificate in the future, simply run certbot-auto again. To
   non-interactively renew *all* of your certificates, run
   "certbot-auto renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le

$ cd /etc/letsencrypt/live/wang123.net
$ ls
README  cert.pem  chain.pem  fullchain.pem  privkey.pem


```
# HTTPS server
server {
    listen       443 ssl;
    server_name  wang123.net;

    ssl_certificate      /etc/letsencrypt/live/wang123.net/fullchain.pem;
    ssl_certificate_key  /etc/letsencrypt/live/wang123.net/privkey.pem;

    ssl_session_cache    shared:SSL:1m;
    ssl_session_timeout  5m;

    ssl_ciphers  HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers  on;

    location / {
        root   html;
        index  index.html index.htm;
    }
}
```


https://github.com/certbot/certbot

https://certbot.eff.org/