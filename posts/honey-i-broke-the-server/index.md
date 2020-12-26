---
title: Honey, I broke the server
first-published: 2011-03-12
description: How I killed my web server with a configuration error
---

Here's how I caused 25 minutes of downtime to my web server today.

<!-- read more -->

Upon restart of the `http` deamon, Apache runs an action called `configtest`, which parses all files in `conf.d/` the
name of which ends in `.conf`, and decides if they are valid configuration files. This is defined in your server's
configuration file, in a line that should be something similar to:

    Include conf.d/*.conf

What I didn't know was that the server will refuse to start altogether, if it doesn't like an instruction in those
files, so, not only had I placed textfiles in that folder that were not valid apache config files, but at some point I
recommended this to others also. In my case, the files were combinations of credentials used for authentication by 
`.htaccess` rules, files produced with `htpasswd`.

The problem only came up when I restarted `httpd` to enable new configuration elsewhere. Of course Apache offered a
useful explanation (where `USERNAME` and `aSdFG123456ASdFG` are the username and the hashed password used for access by
`.htaccess`):

    Invalid command 'USERNAME:aSdFG123456ASdFG', perhaps misspelled or 
    defined by a module not included in the server configuration
    Action 'configtest' failed.
    The Apache error log may have more information.
        ...fail!

It seemed like a logical and convenient place to put the credentials' files upon creation, but it made apache refuse to
launch, because I had the inspiration to name those files `something.conf`. I checked some older web servers, with
prehistoric versions of apache -some 5 years old- and I estimate that this is not an issue for those versions, I assume
that maybe older versions of Apache ignored unknown instructions and started anyway.
