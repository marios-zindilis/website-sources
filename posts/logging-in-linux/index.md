---
title: Logging in Linux
Description: Notes on logging daemons on Linux
first-published: 2014-01-28
last-modified: 2015-04-25
---

*   syslogd, previously the de facto standard Linux implementation, 
    used to be the default on Linux distributions.
*   klogd
*   [metalog](http://metalog.sourceforge.net/)

Syslog Servers
--------------

*   [rsyslog](http://www.rsyslog.com/) is a free and open source syslog 
    server, the default on recent Ubuntu and CentOS distributions. 
    Paid-for options include a Windows agent that sends the Event log 
    to an rsyslog server.
*   [syslog-ng](http://www.balabit.com/network-security/syslog-ng/) is 
    a free and open source syslog server, with great configuration 
    options. Commercial extra options include encryption and and a 
    web interface.
*   [LogStash](http://logstash.net/) is free and open source, and 
    combines a syslog server with a web interface for searching and 
    graphing.
*   [Graylog2](http://graylog2.org/) is also free and open source, and 
    like LogStash it combines the functionality of a syslog server with 
    that of an interface to search and graph the data.
*   [Fluentd](http://fluentd.org/) is a syslog server, capable of 
    scaling up massively.

Web Interfaces
--------------

*   [Kibana](http://www.elasticsearch.org/overview/kibana/) is a web 
    interface for logs collected with LogStash or with other data stored 
    in ElasticSearch.
*   [Octopussy] is a web interface with searching and graphing features.
    Installation instructions exist for RedHat- and Debian-based 
    systems.
*   [LogAnalyzer](http://loganalyzer.adiscon.com/) is a web frontend 
    for syslog, with some analysis and reporting capabilities.

Free and Open Source Log Analysis Software
------------------------------------------

*   [LogReport](http://www.logreport.org/) does log analysis and 
    reporting, but it seems that its development has stopped.
*   [LogSurfer](http://www.crypt.gen.nz/logsurfer/) analyzes logs line 
    by line against predefined regular expressions, and can trigger 
    notifications.
*   [Epylog](https://fedorahosted.org/epylog/) is a time-based log 
    analysis tool, which sends reports and alerts by emails. It is a 
    replacement of logwatch.
*   [SEC](http://simple-evcorr.sourceforge.net/) does log analysis with 
    focus on event correlation.
*   [ELSA](http://code.google.com/p/enterprise-log-search-and-archive/) 
    is an analysis and search tool for syslog-ng with MySQL for backend 
    and Sphinx for indexing.
*   [Clarity](https://github.com/tobi/clarity) is a simple web front 
    end for the contents of a directory with log files, with grep-like 
    and tail-f-like features.

Non-Free or Closed Source or Commercial Log Analysis Software
-------------------------------------------------------------

*   [LogZilla](http://www.logzilla.net/) is free of charge for up to 10 
    devices and up to 1 million messages per day. Beyond those limits, 
    the price scales up according to the selected features. 
    Documentation includes instructions for RedHat- and Debian-based 
    systems.
*   [Splunk](http://www.splunk.com/) is free for up to 500 MBytes of 
    data per day. Download options include packages for 2.6+ Kernel 
    Linux distributions.
*   [CloudPelican](http://www.cloudpelican.com/) is still in development 
    as of this writing. Their website mentions that there is a free 
    version, but downloading the demo requires registration.
*   [XPOLog](http://www.xpolog.com/) is a freeware log analysis software 
    with a standalone web server.
*   [HP ArcSight Logger](http://www8.hp.com/us/en/software-solutions/software.html?compURI=1314386) 
    is a log analysis commercial solution by HP.
*   [LogScape](http://www.logscape.com/) is a Linux based log analysis 
    and indexing tool, with a free basic version.
*   [Sumo Logic](https://www.sumologic.com/) cloud based log management 
    and analytics. Free version works for up to 500 MBytes of data per 
    day, up to three users and up to 7 days retention.
*   [Sawmill](http://www.sawmill.co.uk/) is a closed source analysis 
    tool, with free 30-day demo versions.
*   [Loggly](https://www.loggly.com/) offers log management, analysis 
    and graphing. There is a free version for up to 200 MBytes of data 
    per day and 7-day retention.
*   [Otus SIEM](http://www.bitsteer.com/otus_about.html)
*   [LogRhythm](http://www.logrhythm.com/) log management, analysis, 
    SIEM, focused on security and forensics.


Analytics and Analysis
----------------------

*   [List of web analytics software](http://en.wikipedia.org/wiki/List_of_web_analytics_software)
*   [Log Analysis](http://www.dmoz.org/Computers/Software/Internet/Site_Management/Log_Analysis/) at DMOZ and 
    [Log Analysis](http://dir.yahoo.com/business_and_economy/business_to_business/communications_and_networking/internet_and_world_wide_web/network_management/traffic_management/log_analysis_tools/titles/) at Yahoo.


For Windows
-----------

*   [Snare Backlog](http://www.intersectalliance.com/projects/SnareBackLog/index.html) 
    is a logging server for Windows, that can collect data from several 
    standard sources, as well as from a wide range of operating systems 
    that run its agent.
*   [LogFaces](http://www.moonlit-software.com/logfaces/web/index.php) 
    is a syslog server for Windows.
*   [Aonaware Syslog Server](http://www.aonaware.com/syslog.htm) is also 
    a syslog server, both free and open source.
*   [Log Parser](http://www.microsoft.com/en-us/download/details.aspx?id=24659) 
    is a search tool for logs and other data sources.

See Also
--------

*   [How to Setup Logstash on Linux with ElasticSearch, Redis, Nginx](http://www.thegeekstuff.com/2014/12/logstash-setup/)
*   [ELKs in LateRooms](http://engineering.laterooms.com/elks-in-laterooms/)
*   [What are good open-source log monitoring tools on Linux](http://xmodulo.com/open-source-log-monitoring-tools-linux.html)
