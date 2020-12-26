---
language: el
title: Subversion (ελληνικά)
description: Σημειώσεις για το Subversion
Author: Marios Zindilis
first-published: 2011-07-02
last-modified: 2014-05-01
---

Μερικές οδηγίες για την εγκατάσταση και λειτουργία του Subversion. Οι 
οδηγίες είναι για Ubuntu 10.10, μπορεί να λειτουργούν αυτούσιες ή 
παρόμοιες και σε άλλες εκδόσεις ή διανομές.

Εγκατάσταση του Subversion:

```bash
sudo apt-get install subversion
```

Στη συνέχεια, βρείτε το URL που θέλετε να κάνετε checkout, π.χ: 
http://mediawiki.googlecode.com/svn/trunk/extensions/geshi/. 
Μεταβείτε στο φάκελο που θέλετε να κατεβάσετε τα αρχεία:

```bash
cd Downloads
```

και εκτελέστε:

```bash
svn checkout http://mediawiki.googlecode.com/svn/trunk/extensions/geshi/
```

Όλα τα αρχεία κάτω από τον φάκελο 
http://mediawiki.googlecode.com/svn/trunk/extensions/geshi/ 
θα έχουν τώρα αντιγραφεί στον τοπικό φάκελο Downloads/geshi. 
