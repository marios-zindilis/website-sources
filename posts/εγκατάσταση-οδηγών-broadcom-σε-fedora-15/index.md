---
title: Εγκατάσταση οδηγών Broadcom σε Fedora 15
Author: Marios Zindilis
first-published: 2011-05-07
---

Πολλοί φορητοί υπολογιστές έχουν κάρτες της Broadcom για σύνδεση σε ασύρματα δίκτυα. Οι οδηγοί υλικού της Broadcom διατίθενται μεν δωρέαν, αλλά είναι κλειστού κώδικα, κι έτσι δεν συμπεριλαμβάνονται στην διανομή Fedora. 

<!-- read more -->

Μπορείτε όμως εύκολα να τους εγκαταστήσετε:
<ol><li>Θα χρειαστείτε τα προγράμματα <strong>b43-fwcutter</strong> και <strong>wget</strong> τα οποία είναι προεγκατεστημένα στο Fedora. Σε περίπτωση που λείπουν από το σύστημά σας, εγκαταστήστε τα εκτελώντας:<pre>su
yum install b43-fwcutter wget</pre></li>
<li><strong>Κατεβάστε</strong> τους οδηγούς (υπάρχει και μία νεότερη έκδοση, η broadcom-wl-4.150.10.5.3.tar.bz2, η οποία παρουσιάζει κάποια σφάλματα στο τέλος της διαδικασίας):<pre>wget http://downloads.openwrt.org/sources/broadcom-wl-4.150.10.5.tar.bz2</pre></li>
<li><strong>Αποσυμπιέστε</strong> το αρχείο (θα δημιουργηθεί ένας φάκελος με το όνομα <code>broadcom-wl-4.150.10.5</code>):<pre>tar xjf broadcom-wl-4.150.10.5.tar.bz2</pre></li>
<li>Μπείτε στον φάκελο:<pre>cd broadcom-wl-4.150.10.5/driver</pre></li>
<li>Τέλος, με το b43-fwcutter θα πάρετε τα τμήματα του οδηγού που χρειάζεται το Fedora 15 και θα τα εγκαταστήσετε στο <code>/lib/firmware</code>:<pre>b43-fwcutter -w /lib/firmware/ wl_apsta_mimo.o</pre></li>
</ol>
Αυτό ήτανε, αν όλα έχουν λειτουργήσει σωστά, θα μπορείτε να δείτε τα διαθέσιμα ασύρματα δίκτυα που βρίσκονται κοντά σας. Αν δεν έχουν όλα λειτουργήσει σωστά τότε… διαβάστε την τεκμηρίωση, ξεκινώντας από <a href="http://linuxwireless.org/en/users/Drivers/b43#device_firmware_installation">εδώ</a>.
