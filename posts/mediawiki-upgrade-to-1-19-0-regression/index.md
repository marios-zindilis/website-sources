---
title: MediaWiki upgrade to 1.19.0 regression
Author: Marios Zindilis
first-published: 2012-05-11
description: How to work around a regression in the 1.19.0 version of MediaWiki
---

Upgrade from an earlier version to 1.19.0 breaks some installations, with error:

<pre>"Revision::fetchFromConds". Database returned error "1054:
Unknown column 'rev_sha1' in 'field list' (localhost)".</pre>

This has been <a href="https://bugzilla.wikimedia.org/show_bug.cgi?id=34310" title="MediaWiki Bugzilla BUG 34310">reported as a bug</a>, and <a href="https://www.mediawiki.org/wiki/Special:Code/MediaWiki/111123" title="MediaWiki Patch 111123 for Bug 34310">fixed in the development branch</a>. Temporary fixes: downgrade back to <a href="http://download.wikimedia.org/mediawiki/1.18/mediawiki-1.18.3.tar.gz" title="MediaWiki 1.18.3 Tarball">1.18.3 (tar.gz)</a> or upgrade to a snapshot of the master branch of <a href="https://toolserver.org/~krinkle/mwSnapshots/" title="MediaWiki development snapshots">1.20alpha</a>.
