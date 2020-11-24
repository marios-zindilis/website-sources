---
title: Εγκατάσταση Guake σε CentOS6
Author: Marios Zindilis
first-published: 2011-11-19
---

Το <a href="http://guake.org/" title="Guake Terminal">Guake</a> είναι ένα μικρό εργαλείο το οποίο κάθεται στην περιοχή ειδοποιήσεων του GNOME2 (ή του GNOME3 με Fallback), και ανοίγει το τερματικό μόνο με την πίεση του πλήκτρου F12. Το βρίσκω εξαιρετικά χρήσιμο εργαλείο, και το έχω εγκατεστημένο σε όλα μου τα συστήματα. 

<!-- read more -->

Πιο κάτω ακολουθούν οδηγίες για την εγκατάσταση του Guake, με 
μεταγλώττιση του πηγαίου κώδικα, σε ένα φρεσκοεγκατεστημένο σύστημα με 
CentOS6, δεδομένου ότι δεν υπάρχει στα προεπιλεγμένα αποθετήρια. Για 
τους πιστούς των <code>.rpm</code>, υπάρχει [στα αποθετήρια EPEL](/docs/centos-6-enable-epel-repository.html).

<h3>Λήψη και αποσυμπίεση</h3>
Η τελευταία έκδοση του πηγαίου κώδικα βρίσκεται στη σελίδα <a href="http://guake.org/downloads" title="Guake Terminal Downloads">Downloads</a>. Τη στιγμή που γράφονται αυτές οι γραμμές είναι η 0.4.2. 
<pre>
[marios@centos6 marios]$ cd Downloads
[marios@centos6 Downloads]$ wget http://guake.org/downloads/3
[marios@centos6 Downloads]$ tar zxvf 3
[marios@centos6 Downloads]$ cd guake-0.4.2
</pre> 
<h3>Εγκατάσταση εξαρτήσεων</h3>
Το Guake χρειάζεται τα πακέτα: <code>gcc</code>, <code>make</code>, <code>intltool</code>, <code>gtk2-devel</code> (παρέχει την εξάρτηση gtk+-2.0) και <code>pygtk2-devel</code> (παρέχει την εξάρτηση pygtk-2.0). Μερικά από αυτά τα πακέτα μπορεί να υπάρχουν ήδη στο σύστημά σας, οπότε το <code>yum</code> απλά θα τα παραβλέψει.
<pre>
[marios@centos6 guake-0.4.2]$ su
Password:
[root@centos6 guake-0.4.2]# yum install gcc make intltool gtk2-devel pygtk2-devel
</pre>
<h3>configure, make και make install</h3>
Σε αυτό το σημείο υπάρχουν όλες οι εξαρτήσεις στο σύστημα και μπορείτε να προχωρήσετε με ρύθμιση, μεταγλώττιση και εγκατάσταση από τον πηγαίο κώδικα. Σημειώστε ότι αν κάνετε αυτή τη δουλειά μέσω απομακρυσμένης πρόσβασης με <code>ssh</code> ή <code>telnet</code>, τότε το <code>configure</code> θα σας εμφανίσει το σφάλμα «<code>could not import gtk</code>». Αυτό οφείλεται στο <a href="https://bugzilla.redhat.com/show_bug.cgi?id=450189" title="RedHat Bugzilla Bug #450189">σφάλμα 450189</a>, το οποίο έχει διορθωθεί στις πρόσφατες εκδόσεις του Fedora αλλά όχι του RHEL ή του CentOS. Επομένως, χρειάζεται να εκτελέσετε τουλάχιστον το <code>configure</code> από τοπική σύνδεση στο ίδιο το σύστημα.
<pre>
[root@centos6 guake-0.4.2]# ./configure
[root@centos6 guake-0.4.2]# make
[root@centos6 guake-0.4.2]# make install
</pre>
Αυτό ήταν, το Guake είναι τώρα εγκατεστημένο και έτοιμο για χρήση. Μπορείτε να το βρείτε στο μενού Applications » Accessories » Guake Terminal. Προσωπικά, προτιμώ να το προσθέτω και στις εφαρμογές που εκκινούν με κάθε νέα συνεδρία μου στο GNOME, από το μενού System » Preferences » Startup Applications.

