---
title: ISO Date Format in Apache Logs
description: How to format the date in Apache logs
first-published: 2014-05-16
---

Apache's `mod_log_config` module (installed by default on CentOS 6) allows for  the `CustomLog` directive, which in
turn takes a log format specification. This post shows how to use ISO format for Apache logs.

<!-- read more -->

The default log format string is usually:

    LogFormat "%h %l %u %t \"%r\" %>s %b" common

In the above line, `common` is the name of the log format, which is later 
references in a `CustomLog` directive, for example:

    CustomLog logs/example.com-access_log common

The meaning of the `%` fields is specified at [the Apache documentation for 
`mod_log_config`](http://httpd.apache.org/docs/2.2/mod/mod_log_config.html). 
What is interesting is that the `%t` parameter can take an optional date 
formatting string, in the form `%{date format}t`. The `date format` part 
should be `strftime(3)`-compatible.

For example, the following date format will produce timestamps like 
`2014-05-16 13:45:32`:

{% raw %}
    LogFormat "%h %l %u %{%Y-%m-%d %H:%M:%S}t \"%r\" %>s %b" example
{% endraw %}
 
You will then need to reference the `example` name of this log format 
specification as:

    CustomLog logs/example.com-access_log example.

The `%` fields of `strftime` are listed in the [man page for 
`strftime`](/docs/man/library-calls/strftime.html)

<hr>

Image credit: Part of *An unidentified sawmill worker with two large logs...*.
[Source](https://archive.org/details/turnbull_v3_737243)