---
title: openDCIM 2.1 Installation on Centos 6
Description: How to install openDCIM 2.1 on CentOS 6.x
first-published: 2013-05-25
last-modified: 2015-03-26
---

<p class="lead">This is a bottom-up guide for installing 
[openDCIM](/docs/opendcim.html) on a system running CentOS 6.x.</p>

This guide is released in the **Public Domain**, except from the section 
"*Enable HTTPS*" which is a snippet from [CentOS Wiki](http://wiki.centos.org/HowTos/Https) 
and is licenced under [Creative Commons Attribution-Share Alike 3.0 Unported License](http://creativecommons.org/licenses/by-sa/3.0/).

## Software Used ##

This guide has been tested with:

*   openDCIM version 2.1, the current version as of the writing of the 
    guide, but it should apply to previous versions, and possibly to 
    future ones as well. 
*   CentOS 6.4 Minimal 64bit, with the latest updates as of 2013-05-25, 
    but it should apply to previous versions, and possibly to future 
    ones as well.
*   The `vim` editor is used in the commands below, because it adds nice 
    colored syntax highlighting when editing configuration files. If you 
    don't have `vim`, you can use `vi`, which comes preinstalled with 
    CentOS, or any text editor of your choice.

Install Apache, PHP, MySQL
--------------------------

Install, start and enable Apache:

    yum -y install httpd
    service httpd start
    chkconfig httpd on

Install PHP, and the MBSTRING module required for internationalization:

    yum -y install php
    yum -y install php-mysql
    yum -y install php-mbstring

Install, start and enable MySQL Server:

    yum -y install mysql-server
    service mysqld start
    chkconfig mysqld on

Secure MySQL Server:

    mysql_secure_installation

During this step, you will:

1. Set a root password
2. Remove anonymous users
3. Disallow root login remotely
4. Remove test database and access to it
5. Reload privilege tables

Create a database for openDCIM (change the database name `dcim` and the 
password `dcimpassword` to something that suits you):

    mysql -u root -p
    mysql> create database dcim;
    mysql> grant all privileges on dcim.* to 'dcim'@'localhost' identified by 'dcimpassword';

Enable HTTPS
------------

Install Apache SSL Module:

    yum -y install mod_ssl

Generate the necessary keys and copy them to the proper directories:

    cd /root
    openssl genrsa -out ca.key 1024 
    openssl req -new -key ca.key -out ca.csr
    openssl x509 -req -days 365 -in ca.csr -signkey ca.key -out ca.crt
    cp ca.crt /etc/pki/tls/certs
    cp ca.key /etc/pki/tls/private/ca.key
    cp ca.csr /etc/pki/tls/private/ca.csr

To let Apache know of the proper locations of the keys, edit 
`/etc/httpd/conf/httpd.conf`...

    vim +/SSLCertificateFile /etc/httpd/conf.d/ssl.conf

... comment out the line...

    SSLCertificateFile /etc/pki/tls/certs/localhost.crt

... and substitute with:

    SSLCertificateFile /etc/pki/tls/certs/ca.crt

Also comment out the line...

    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key

... and substitute with:

    SSLCertificateKeyFile /etc/pki/tls/private/ca.key

To set the server name:

    vim +/ServerName /etc/httpd/conf/httpd.conf

Find the line...

    #ServerName www.example.com:80

...and add below it:

    ServerName opendcim.example.com:443

Finally restart Apache...

    service httpd restart

Create a VirtualHost
--------------------

    vim +/NameVirtualHost /etc/httpd/conf/httpd.conf

Find the line:

    #NameVirtualHost *:80

... and add below it:

    NameVirtualHost *:443

Now create a new configuration file for the openDCIM VirtualHost...

  vim /etc/httpd/conf.d/opendcim.example.com.conf

... add the lines...

    <VirtualHost *:443>
      ServerAdmin marios@zindilis.com
      DocumentRoot /var/www/opendcim
      ServerName opendcim.zindilis.net
      <Directory /var/www/opendcim>
        AuthType Basic
        AuthName "openDCIM"   
        AuthUserFile /var/www/.htpasswd
        Require valid-user
      </Directory>
    </VirtualHost>

Enable User Authentication
--------------------------

You have protected the openDCIM web directory with a requirement for 
Basic authentication, with the lines already added in your Apache 
configuration file above.

Now, to create at least on user, do:

    touch /var/www/.htpasswd
    htpasswd /var/www/.htpasswd Administrator

You will be asked for a password for user "Administrator" twice.

Open Web Access on Firewall
---------------------------

The `iptables` firewall is enabled on CentOS by default, and blocks 
access to HTTPS port 443. To allow it...

    vim /etc/sysconfing/iptables

... find the line...

    -A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT

...and add below it:

    -A INPUT -m state --state NEW -m tcp -p tcp --dport 443 -j ACCEPT

Restart iptables:

    service iptables restart

Download and Install openDCIM
-----------------------------

Download the latest version of openDCIM from [opendcim.com](http://www.opendcim.org/downloads.html) 
(version 2.1 at the time of writing this guide):

    cd /var/www
    curl -O http://www.opendcim.org/packages/openDCIM-2.1.tar.gz

Extract the archive and create a symbolic link:

    tar zxvf openDCIM-2.1.tar.gz
    ln -s openDCIM-2.1 opendcim

The symbolic link is not required. If you don't want to create it, just 
rename the directory `openDCIM-2.1` to `opendcim`. However, having a 
symbolic link in place allows you to find out the version of openDCIM at 
a glance, and makes upgrades easier.

Now, prepare the configuration file for access to the database:

    cd /var/www/opendcim
    cp db.inc.php-dist db.inc.php
    vim db.inc.php

Edit the following lines, to reflect your settings of database host (in 
this example `localhost`), database name (`dcim`), and credentials that 
you assigned when creating the database:

    $dbhost = 'localhost';
    $dbname = 'dcim';
    $dbuser = 'dcim';
    $dbpass = 'dcimpassword';

Finally, restart Apache one last time:

    service httpd restart

Now, you can open openDCIM in your browser to proceed with the 
installation. If you have set up a DNS entry for a domain name to point 
to your web server's IP, you will go to something similar to `https://opendcim.example.com/`. 
Otherwise, you can visit `https://IP_of_web_server/`. 

In any case, you will be asked to accept the web server's HTTPS 
certificate. This will only happen once per browser. You will then be 
asked for the Authentication credentials that you configured earlier 
with `htpasswd`.

To finish with the installation, follow [Scott Milliken's (lead 
developer) video tutorial on YouTube](http://www.youtube.com/watch?v=XzEI6Z8yhRg), 
starting from minute 09:00. He will walk you through the first settings 
to get you started in the web interface.

Additional Resources
---------------------

*   [How to setup HTTPS on CentOS](http://wiki.centos.org/HowTos/Https)
*   [openDCIM Installation Video Tutorial](http://www.youtube.com/watch?v=XzEI6Z8yhRg)
*   [openDCIM INSTALL File on GitHub](https://github.com/samilliken/openDCIM/blob/master/INSTALL)
