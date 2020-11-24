---
title: "Reading #6"
Author: Marios Zindilis
first-published: 2011-03-30
---

Today's reading recommendations.

<!-- read more -->

### Free and Open Source related stuff ###

*  [Yahoo plans to open source code for non-core technologies](http://www.itworld.com/internet/141635/yahoo-plans-open-source-code-non-core-technologies)

### Networking related ###

* [Can you really be traced by your IP?](http://www.pcpro.co.uk/features/366349/can-you-really-be-traced-from-your-ip-address)

### Misc geek reading ###

<ul>
<li><a href="http://www.itworld.com/security/139794/with-hacking-music-can-take-control-your-car">Hacking 
into a car from an infected .mp3</a>.</li>
<li>From InfoWorld.com I enjoy reading articles by 
<a href="http://www.infoworld.com/blogs/paul-venezia">Paul Venezia</a>, and by 
<a href="http://infoworld.com/blogs/anonymous/">Anonymous</a>. Today I read 
<a href="http://www.infoworld.com/print/148993">Cloud computing makes users of 
us all</a>, <a href="http://www.infoworld.com/print/140239">What the days 
of gonzo IT taught us</a>, <a href="http://www.infoworld.com/print/131012">Whatever 
happened to Perl?</a> (quote: "Perl makes easy things hard, and hard things possible"), 
<a href="http://www.infoworld.com/print/155616">Dealing with system 
consoles from hell</a>, and <a href="http://www.infoworld.com/print/156031">Danger, 
danger! Newbies at work!</a>.</li>
<li><a href="http://techie-buzz.com/tech-news/mysql-com-database-compromised-sql-injection.html">mysql.com, SQL injected</a></li>
<li>Not as fun a read as the writer might have thought, but here's <a href="http://www.pcpro.co.uk/blogs/2011/03/08/a-letter-on-behalf-of-the-worlds-pc-fixers/">A letter on behalf of the world's PC fixers</a>. I conclude that the guy just never got any.</li><li>How <a href="http://www.stormdriver.com/blog/start-ups-in-the-maze-of-software-patents/">software patents choke startups</a>, how an <a href="http://www.techdirt.com/articles/20081107/0118162765.shtml">oilfield services corporation is patenting patent-trolling</a> and how to <a href="http://patentabsurdity.com/download.html">download a free video that explains some of this crazy shit</a>.</li>
<li><a href="http://codex.wordpress.org/Changing_File_Permissions">Changing file permissions in WordPress</a>.</li></ul>

Regarding that last point, I did some testing and found that the minimum permissions for a php file to execute via the browser are 004, or -------r-- since php is running as "other", or in my case as www-data. The "usual" permissions for php files are 644 or -rw-r--r--.

Now, the minimum permissions of the containing folder, are 001, or d--------x, but some scripts will not work if they are trying to read something in their own folder, or in another folder that has 001 permissions. The "usual" permissions for folders containing php files are 755 or drwxr-xr-x.

In a production environment, one may reduce permissions to some files, e.g. a config.php, to disallow unnecessary access.
