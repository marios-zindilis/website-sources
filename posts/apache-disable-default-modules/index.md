---
title: Disabling Apache Default Modules
description: Which default Apache modules can be disabled
Author: Marios Zindilis
first-published: 2013-11-12
last-modified: 2014-10-05
---

With a default installation of Apache running, a set of modules are configured 
to load automatically, to serve what the server's and the distribution's 
developers evaluated to be the most common needs. These modules can be (and 
probably should be) disabled on a production web server. This might save a very 
tiny bit of memory for each module, and also can improve security, in case a 
vulnerability exists in one of the loaded but unused modules.

In fact, if you want to be paranoid about security, you can pretty much disable 
all Apache modules, and only selectively enable those that are required for the 
functionality that you want your server to have.

In CentOS, you can see which modules are loaded on a running instance of 
Apache, with:

    httpd -M

`mod_proxy` and related proxy modules
-------------------------------------

The `mod_proxy` and its related modules add the capability to use Apache as a 
proxy server in front of other Web servers, as well as in front of FTP servers, 
application servers, FastCGI servers, and other types of servers specified by 
loading additional modules.

These are not necessary in a setup where Apache is used as a standalone Web 
server. You can disable these features by commenting out the lines:

        LoadModule proxy_module modules/mod_proxy.so
        LoadModule proxy_balancer_module modules/mod_proxy_balancer.so
        LoadModule proxy_ftp_module modules/mod_proxy_ftp.so
        LoadModule proxy_http_module modules/mod_proxy_http.so
        LoadModule proxy_ajp_module modules/mod_proxy_ajp.so
        LoadModule proxy_connect_module modules/mod_proxy_connect.so

The `mod_proxy_ajp` module provides the ability to proxy HTTP requests 
coming to the web server to an application server. It is commonly used 
together with Apache Tomcat. Read more about the [Apache Module `mod_proxy_ajp`](http://httpd.apache.org/docs/current/mod/mod_proxy_ajp.html).

`mod_substitute`
----------------

The `substitute` module allows for sed-like regex-based string substitutions in 
HTTP responses. To disable it, comment out the line:

    LoadModule substitute_module modules/mod_substitute.so

Read more about the [Apache Module `mod_substitute`](http://httpd.apache.org/docs/current/mod/mod_substitute.html)

`mod_userdir`
-------------

Module `userdir` is used to serve web content from inside users' home 
directories, and it is loaded but deactivated (with the `UserDir disabled` 
directive) by default on CentOS 6. To unload it permanently, edit 
`/etc/httpd/conf/httpd.conf`, and comment out the line:

        LoadModule userdir_module modules/mod_userdir.so

You might also comment out the lines in the same file:

        <IfModule mod_userdir.c>
            UserDir disabled
        </IfModule>

`mod_dav` and `mod_dav_fs`
--------------------------

Both of these modules provide support for [WebDav](http://www.webdav.org/) and 
can be disabled if WebDav is not going to be used in production. To disable, 
comment out the lines:

        LoadModule dav_module modules/mod_dav.so
        LoadModule dav_fs_module modules/mod_dav_fs.so

You might also comment out the lines in the same file:

        <IfModule mod_dav_fs.c>
            DAVLockDB /var/lib/dav/lockdb
        </IfModule>

`mod_speling`
-------------

The `speling` module tries to guess if a visitor has misspelled the URL of a 
page, and returns the page anyway if the requested misspelled page name and the 
actual page name are close enought. To disable it, comment out the line:

        LoadModule speling_module modules/mod_speling.so

Read more about the [Apache `speling` module](https://httpd.apache.org/docs/current/mod/mod_speling.html)

`mod_version`
-------------

The `version` module allows you to specify different configuration depending on 
the version of Apache that is running, by defining that configuration inside 
`<IfVersion>` blocks of code. This makes it possible to have the same basic 
configuration file for many servers, which can be useful to some organizations. 
To disable this feature, commend out the line:

        LoadModule version_module modules/mod_version.so

Authentication and authorization modules
----------------------------------------

Apache provides a few ways to handle user authentication (who can do something) 
and authorization (what they can do). This functionality is provided by modules 
that contain `authn` or `authz` in their names, repsectively. If you don't plan 
on delegating these features to Apache at all, you can disable all related 
modules:

*   `mod_ldap` and `mod_authnz_ldap`: These two modules can be used to delegate 
     authentication to an LDAP directory. To disable, comment out these lines:

        LoadModule ldap_module modules/mod_ldap.so
        LoadModule authnz_ldap_module modules/mod_authnz_ldap.so

*   `mod_auth_basic` and `mod_auth_digest` offer Basic or Digest authentication 
    respectively, and are used in combination with `mod_authn_file` (which 
    allows the definition of a file that contains user credentials, and makes 
    the use of the `htpasswd` command possible),`mod_authz_user` (which allows 
    for different access in different parts of a website, and makes the use of 
    the `Require` directive possible), and `mod_authz_groupfile` (which allows 
    for access to be restricted based on group membership, usually defined in a 
    `htgroup` file). Also, `mod_authn_default` and `mod_authz_default` are used 
    to reject credentials if authentication or authorization are enabled but 
    not configured. To disable, comment out these lines:

        LoadModule auth_basic_module modules/mod_auth_basic.so
        LoadModule auth_digest_module modules/mod_auth_digest.so
        LoadModule authn_file_module modules/mod_authn_file.so
        LoadModule authz_user_module modules/mod_authz_user.so
        LoadModule authn_default_module modules/mod_authn_default.so
        LoadModule authz_groupfile_module modules/mod_authz_groupfile.so
        LoadModule authz_default_module modules/mod_authz_default.so

*   `mod_authn_alias` allows you to create multiple Basic authentication 
    providers, and give them names (aliases) that are then used in the rest of 
    the configuration. To disable, comment out:

        LoadModule authn_anon_module modules/mod_authn_anon.so

*   `mod_authn_anon` allows anonymous users to access an otherwise restricted 
    website. In combination with some authorization modules, it can display 
    different content to authenticated and anonymous users. To disable, comment 
    out:

        LoadModule authn_anon_module modules/mod_authn_anon.so

*   `mod_authn_dbm` is an alternative to using an `htpasswd` file as the users' 
    database, and scales better when the number of users exceeds 200. To 
    disable, comment out:

        LoadModule authn_dbm_module modules/mod_authn_dbm.so

*   `mod_authz_host` allows restriction of access based on miscellaneous 
    properties of the client, as those are capture in Apache's environment 
    variables, the most common of which are the IP and/or hostname of the 
    client, and the web request sent by it. Access is controlled with the 
    `Allow` and `Deny` directives, and the order of application is controlled 
    with the `Order` directive. To disable, comment out:

        LoadModule authz_host_module modules/mod_authz_host.so

    If you disable this module, you will also have to comment out any uses of 
    the combination `Order`, `Allow` and `Deny` in your configuration, since 
    Apache will no longer understand their meaning. Running either `httpd -t` 
    or `service httpd configtest` after commenting out the module will tell you 
    the instances of those directives.

    Note that one of the uses of `mod_authz_host` is to disallow clients from 
    reading `.htaccess` files. If you disable this module, you will either need 
    to replace the instructions in `.htaccess` files with configuration inside 
    the main `httpd.conf`, or to find another way to make those files 
    unreadable.

*   `mod_authz_owner` will authorize access to a user by comparing the username 
    provided to either Basic or Digest authentication with the ownership of the 
    file requested on the underlying filesystem. To disable, comment out:

        LoadModule authz_owner_module modules/mod_authz_owner.so
