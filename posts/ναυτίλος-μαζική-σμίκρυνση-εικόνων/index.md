---
title: "Ναυτίλος: Μαζική σμίκρυνση εικόνων"
Author: Marios Zindilis
first-published: 2011-06-29
description: Βήμα προς βήμα οδηγός δημιουργίας ενός πρόσθετου του Ναυτίλου για σμίκρυνση εικόνων
---

Σε αυτό το άρθρο περιγράφω τη δημιουργία μιας εντολής για μαζική σμίκρυνση εικόνων, η οποία εμφανίζεται όταν κάνουμε
δεξί κλικ σε ένα παράθυρο του Ναυτίλου.

<!-- read more -->

![Μενού του Ναυτίλου](nautilus-resizeall-0.png)

Έφτιαξα ένα σενάριο εντολών για τον περιηγητή αρχείων του GNOME, τον Ναυτίλο, το οποίο σμικρύνει μαζικά όλες τις
εικόνες με επεκτάσεις `.jpg`, `.jpeg` ή `.png`.

Χρησιμοποιεί το εργαλείο `convert`, από το πακέτο ImageMagick, άρα 
μπορεί να τροποποιηθεί ώστε να σμικρύνει οποιονδήποτε τύπο εικόνας 
υποστηρίζεται από το ImageMagick. Επίσης, χρησιμοποιεί το Zenity για 
την προβολή αναδυόμενων παραθύρων στον χρήστη: αν δεν υπάρχει το 
ImageMagick θα τσινίσει, αν όμως υπάρχει τότε θα σας ρωτήσει για:

1.  το <strong>όνομα του φακέλου</strong> μέσα στο οποίο θα μπουν οι 
    μικρότερες εικόνες (από προεπιλογή είναι «thumbs»), και
2.  το <strong>ποσοστό</strong> κατά το οποίο θα σμικρυνθούν οι 
    εικόνες (από προεπιλογή είναι 50%).

Τέλος, εμφανίζει μια μπάρα προόδου της διαδικασίας σμίκρυνσης.

Για να το χρησιμοποιήσετε, <strong>αντιγράψτε το αρχείο</strong> στον 
κρυφό φάκελο <code>.gnome2/nautilus-scripts/</code> μέσα στον Προσωπικό 
σας φάκελο. Μετά την αντιγραφή, πρέπει να <strong>αλλάξετε τα δικαιώματά 
του</strong>, και να το κάνετε εκτελέσιμο. Στο εξής, θα εμφανίζεται στο 
μενού του Ναυτίλου, το οποίο αναδύεται όταν πατήσετε το δεξί κλικ μέσα 
σε έναν φάκελο ή πάνω σε ένα αρχείο. 

Κατεβάστε το [σενάριο εντολών από έδω](https://github.com/marios-zindilis/Scripts/blob/master/Bash/ResizeAll) (δεξί κλικ και
αποθήκευση, αν προσπαθήσετε να το δείτε στον browser τα ελληνικά γράμματα 
μάλλον δεν θα εμφανίζονται). Το σενάριο έχει δοκιμαστεί σε Ubuntu 10.10 με 
GNOME 2, και σε Fedora 15 με GNOME 3.

<div class="container">
<figure class="col-md-4">
<a href="/static/img/nautilus-resizeall-1.png">
<img src="/static/img/nautilus-resizeall-1.png" alt="Καθορισμός φακέλου προορισμού" title="Καθορισμός φακέλου προορισμού" />
</a>
<figcaption>Φάκελος προορισμού</figcaption>
</figure>

<figure class="col-md-4">
<a href="/static/img/nautilus-resizeall-2.png">
<img src="/static/img/nautilus-resizeall-2.png" alt="Ποσοστό σμίκρυνσης" title="Ποσοστό σμίκρυνσης" /> 
</a>
<figcaption>Ποσοστό σμίκρυνσης</figcaption>
</figure>

<figure class="col-md-4">
<a href="/static/img/nautilus-resizeall-3.png">
<img src="/static/img/nautilus-resizeall-3.png" alt="Μπάρα προόδου" title="Μπάρα προόδου" />
</a>
<figcaption>Μπάρα προόδου</figcaption>
</figure>
</div>

### Μερικές ακόμα σημειώσεις ###

*   Το εγχειρίδιο του Zenity υπάρχει [μεταφρασμένο στα Ελληνικά](https://help.gnome.org/users/zenity/2.32/) 
    χάρη στην προσπάθεια του Στέριου Π.
*   Τα αναδυόμενα παράθυρα του Zenity, δεν βγαίνουν πάντα μπροστά από 
    τα υπόλοιπα παράθυρα στην επιφάνεια εργασίας. Αυτό ίσως κάνει 
    κάποιους χρήστες να περιμένουν για κάποια ανάδραση από το πρόγραμμα, 
    ενώ το πρόγραμμα έχει ήδη βγάλει το παράθυρο πίσω από κάποιαν άλλη 
    εφαρμογή. Το θέμα έχει συζητηθεί σε αναφορές σφάλματος στο <a href="https://bugzilla.gnome.org/show_bug.cgi?id=448946">GNOME (σφάλμα 448946)</a> και στο <a href="https://bugs.launchpad.net/ubuntu/+source/zenity/+bug/272083">Launchpad (σφάλμα 272083)</a>. Στο δεύτερο, υπάρχουν και patches, ξεπερασμένα τώρα πια -λόγω απεξάρτησης από το Glade. Οι αναφορές σφάλματος είναι κλειστές, μετά από απόφαση του προγραμματιστή ότι αυτή είναι η επιθυμητή λειτουργία του προγράμματος.
