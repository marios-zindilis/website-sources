---
title: Running multiple instances of Thunderbird
Author: Marios Zindilis
first-published: 2012-09-09
description: How to run multiple instances of Thunderbird with different profiles
---

You can run multiple instances of Thunderbird with different user profiles for each instance, by launching it from command line as:

<pre>[marios@bate ~]$ thunderbird -no-remote -P work &
[marios@bate ~]$ thunderbird -no-remote -P personal &</pre>

The <code>-P</code> option defines the profile name. If you want to create a new profile, use <code>-P</code> without a name.

NOTE: This article is a short version of <a href="http://kb.mozillazine.org/Run_multiple_copies_of_Thunderbird_at_the_same_time">Run multiple copies of Thunderbird at the same time</a> from the <a href="http://kb.mozillazine.org/">Mozilla Knowledge Base</a>.
