---
title: Apache host configuration for a Django website
first-published: 2020-12-26
description: How to configuration a virtual host in Apache for a Django website
---

This is the Apache virtual host configuration for an older project that I don't use any more.

<!-- read more -->

This configuration for port 80 redirects everything to HTTPS:

```
<VirtualHost *:80>
    ServerName x.zindilis.com
    DocumentRoot /home/marios/Code/x.zindilis.com
    Redirect / https://x.zindilis.com/
    RewriteEngine on
    RewriteCond %{SERVER_NAME} =x.zindilis.com
    RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]
</VirtualHost>
```

This is the configuration for the virtual host listening to port 443. In addition to the Django default authentication,
this host also had Apache basic authentication to access the website:

```
<VirtualHost *:443>
    ServerName x.zindilis.com
    DocumentRoot /home/marios/Code/x.zindilis.com
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
    Alias /static/ /home/marios/Code/x.zindilis.com/x/static/
    WSGIScriptAlias / /home/marios/Code/x.zindilis.com/wsgi.py
    WSGIDaemonProcess x.zindilis.com processes=2 threads=15 display-name=%{GROUP} python-path=/home/marios/Code/x.zindilis.com
    WSGIProcessGroup x.zindilis.com
    <Directory "/home/marios/Code/x.zindilis.com">
        AuthType Basic
        AuthName "Restricted Content"
        AuthUserFile /etc/apache2/.htpasswd
        Require valid-user

        AllowOverride All
        Options FollowSymlinks
    </Directory>
    SSLCertificateFile /etc/letsencrypt/live/x.zindilis.com/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/x.zindilis.com/privkey.pem
</VirtualHost>
```

Image credit: *Gardens Of Ireland - Beautiful stairs in Dún Laoghaire, Ireland*. In the public domain. 
[Source](https://www.publicdomainpictures.net/en/view-image.php?image=181320&picture=gardens-of-ireland)
