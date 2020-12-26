---
language: el
title: Εγκατάσταση Apache, MySQL και PHP σε Ubuntu 10.10
description: Οδηγίες για την εγκατάσταση Apache, MySQL και PHP σε Ubuntu 10.10
Author: Marios Zindilis
first-published: 2011-10-28
---

Βήματα για εγκατάσταση της στοίβας λογισμικού σε Ubuntu 10.10:

Εγκατάσταση Apache 2
--------------------

    sudo apt-get install apache2

Εγκατάσταση PHP 5
-----------------

    sudo apt-get install php5

Σε προηγούμενες εκδόσεις του Ubuntu χρειαζόταν ξεχωριστά και εγκατάσταση του libapache2-mod-php5, το οποίο πλέον εγκαθίσταται αυτόματα μαζί με την PHP.

Εγκατάσταση MySQL
-----------------

    sudo apt-get install mysql-server

Κατά τη διάρκεια της εγκατάστασης θα ζητηθεί το συνθηματικό του root, του προεπιλεγμένου χρήστη της MySQL.

    sudo apt-get install libapache2-mod-auth-mysql
    sudo apt-get install php5-mysql
