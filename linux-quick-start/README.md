README.md


查看Linux内核版本

* uname -a

    $ uname -a
    Linux homestead 4.4.0-81-generic 
    #104-Ubuntu SMP Wed Jun 14 08:17:06 UTC 2017 
    x86_64 x86_64 x86_64 GNU/Linux


* cat /proc/version

    $ cat /proc/version
    Linux version 4.4.0-81-generic (buildd@lgw01-02) 
    (gcc version 5.4.0 20160609 (Ubuntu 5.4.0-6ubuntu1~16.04.4) ) 
    #104-Ubuntu SMP Wed Jun 14 08:17:06 UTC 2017

* sw_vers (仅适用于macOS)

    $ sw_vers
    ProductName:    Mac OS X
    ProductVersion: 10.12.5
    BuildVersion:   16F73

查看Linux系统版本

* lsb_release -a

    $ lsb_release -a
    No LSB modules are available.
    Distributor ID: Ubuntu
    Description:    Ubuntu 16.04.2 LTS
    Release:    16.04
    Codename:   xenial

    $ lsb_release -a
    No LSB modules are available.
    Distributor ID: Ubuntu
    Description:    Ubuntu 14.04.6 LTS
    Release:        14.06
    Codename:       trusty

* cat /etc/issue (适合Linux发行版)

    $ cat /etc/issue
    Ubuntu 16.04.2 LTS \n \l

* cat /etc/redhat-release 只适合Redhat系的Linux

