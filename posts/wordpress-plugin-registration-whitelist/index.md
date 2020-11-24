---
title: "Πρόσθετο WordPress: Αντιστοίχιση διεύθυνσης email με Λευκή λίστα κατά την εγγραφή νέου χρήστη"
first-published: 2011-04-15
---

Με αφορμή μια ερώτηση από χρήστη στο [ελληνικό φόρουμ του WordPress](http://www.wordpress.gr/forum/thread-779.html), 
έφτιαξα ένα σχετικά απλό πρόσθετο για το WordPress, με το οποίο ο διαχειριστής 
δημιουργεί μία λευκή λίστα με domains, σύμφωνα με τα οποία ελέγχεται η 
ηλεκτρονική διεύθυνση του χρήστη κατά την εγγραφή. Αν η διεύθυνση δεν ανήκει 
στην Λευκή λίστα, τότε η εγγραφή απορρίπτεται.

Το αρχείο βρίσκεται στο <a href="/pub/briki-wp-registration-whitelist/">Pub » Briki WordPress Registration Whitelist</a>, και η εγκατάστασή του δεν διαφέρει από άλλα πρόσθετα του WordPress: αποσυμπίεση, μεταφορά στον φάκελο Plugins, ενεργοποίηση από το Dashboard. Στη συνέχεια εμφανίζεται ένα νέο μενού με το όνομα Registration Whitelist, όπου ο διαχειριστής καθορίζει τη λίστα με τα domains.

Με ελάχιστο κόπο το πρόσθετο μπορεί να μετατραπεί και σε Blacklist, να κάνει δηλαδή την αντίστροφη δουλειά. Αν υπάρχει ενδιαφέρον από κάποιον θα το φτιάξω, αλλά αυτή τη στιγμή έχω ψοφήσει στην κούραση. Για τις ανάγκες της δημιουργίας του πρόσθετου, εκτός από τον πηγαίο κώδικα μερικών άλλών πρόσθετων, διάβασα τα εξής:
<ul><li><a href="http://codex.wordpress.org/Writing_a_Plugin">Codex: Writing a Plugin</a></li>
<li><a href="http://codex.wordpress.org/Creating_Tables_with_Plugins">Codex: Creating tables with Plugins</a></li>
<li><a href="http://codex.wordpress.org/Function_Reference/add_options_page">Function: add_options_page()</a></li>
<li><a href="http://codex.wordpress.org/Function_Reference/add_action">Function: add_action()</a></li>
<li><a href="http://codex.wordpress.org/Administration_Menus">Codex: Administration Menus</a></li>
<li><a href="http://codex.wordpress.org/Plugin_API/Action_Reference">Codex: Action reference</a></li></ul>
