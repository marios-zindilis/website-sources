---
title: Basic Authentication with Apache 2
Description: Basic authentication with Apache 2
first-published: 2013-11-12
last-modified: 2018-01-27
---

To enable **basic authentication** with Apache 2:

1.  Create a set of credentials for the user, using `htpasswd`. The 
    syntax is:

        htpasswd /path/to/htpasswd/file username

    If the `htpasswd` file does not exist yet, pass the additional 
    option `-c` to create it. For example:

        htpasswd /etc/httpd/htpasswd marios

2.  Enter the user password twice. This will create an entry in that 
    file, with a set of credentials, e.g:

        [marios@centos6 ~]$ grep marios /etc/httpd/htpasswd 
        marios:3RoxtKn6QL9Uw

3.  Basic authentication can be applied to a directory on the web 
    server and its subdirectories with the `Directory` Apache directive, 
    or to a URL and whatever follows it, with the `Location` directove. 
    To apply it to a directory, add to your Apache configuration 
    something like:

        <Directory /var/www/html/protected>
            AuthName "Protected"
            AuthType Basic
            AuthUserFile /etc/httpd/htpasswd
            Require user marios
        </Directory>

    If you have multiple users, you can specify `Require valid-user`, 
    in which case all users with credentials in the `AuthUserFile` 
    will be allowed to login.

Optionally, you can further secure that specific directory or location 
with an `AllowFrom` directive, and restrict the ranges of IP addresses 
from which the directory or location will be accessible.

## Basic Authentication Against Active Directory ##

To authenticate and authorize users against Active Directory, you will need to
create an Active Directory user that will be used to bind to AD. In the
following example, the user is `apache_authentication_user` and its password is
`apache_authentication_password`.

```    
AuthType Basic
AuthName "Authorized Users Only"
AuthBasicProvider ldap
AuthzLDAPAuthoritative on
AuthLDAPBindDN "CN=apache_authentication_user,OU=system_accounts,DC=example,DC=com"
AuthLDAPBindPassword apache_authentication_password
AuthLDAPGroupAttributeIsDN on
AuthLDAPURL "ldap://domain-controller.example.com:389/OU=USERS,DC=example,DC=com?sAMAccountName?sub?(objectClass=*)"
<Limit GET POST>
    require ldap-group CN=sysadmins,OU=GROUPS,DC=example,DC=com
    require ldap-group CN=finance,OU=GROUPS,DC=example,DC=com
</Limit>
```

See Also
--------

*   [Apache Web Login Authentication](http://www.yolinux.com/TUTORIALS/LinuxTutorialApacheAddingLoginSiteProtection.html), 
    a tutorial with many Apache authentication examples, including Basic.
