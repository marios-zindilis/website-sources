---
title: Login in to website with Digest authentication in Python
Author: Marios Zindilis
first-published: 2012-08-15
tags:
- Python
---

Websites that use HTTP authentication, can use one of a couple of 
authentication methods, like Basic or **Digest**. When first opened, the 
website returns a 401 message, along with a string that contains the 
type of authentication and the **Realm**, and expects from the user to 
enter their credentials. Here is how to login with Python to a website 
that uses Digest.

<!-- read more -->

First, verify the authentication type and get the Realm with 
`curl`. The option `-I` only fetches the headers:

```bash
[marios@zindilis.net]$ curl -I http://example.com
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Digest realm="Example Realm",  nonce="1a7278f234efe7894dfd823", algorithm=MD5, qop="auth"
```

The significant parts from the output above is that the HTTP 
Authentication method is `Digest` and that the realm is 
`Example Realm`.

Now the Python part:

```python
import urllib2

URL      = 'http://example.com'
Realm    = 'Example Realm'
Username = 'marios'
Password = 'p@ssw0rd'

authhandler = urllib2.HTTPDigestAuthHandler()
authhandler.add_password(Realm, URL, Username, Password)
opener = urllib2.build_opener(authhandler)
urllib2.install_opener(opener)
page_content = urllib2.urlopen(URL)
```

The `page_content` variable now contains the contents of the 
webpage returned after the authentication, and can be read with 
something like `for line in page_content` or parsed as 
HTML/XML.
