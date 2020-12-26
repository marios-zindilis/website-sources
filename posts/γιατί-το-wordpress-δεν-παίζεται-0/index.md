---
title: "Γιατί το WordPress δεν παίζεται (#0)"
Author: Marios Zindilis
first-published: 2011-03-12
description: Εμφάνιση περιεχομένου μόνο σε συνδεδεμένους χρήστες στο WordPress
---

Το WordPress δεν παίζεται! Για πολλούς λόγους, αλλά να ένας συγκεκριμένος.

<!-- read more -->

Αφού έκανα μερικές αλλαγές στο 
<a href="http://www.rodrigogalindez.com/themes/modernist/">εξαίσιο 
οπτικό θέμα Modernist</a> από τον 
<a href="http://www.rodrigogalindez.com/">Rodrigo Galindez</a>, μπόρεσα 
να φτιάξω έναν υποκατάλογο στον εξυπηρετητή που φιλοξενεί αυτό εδώ το 
ιστολόγιο, με το εξής περιεχόμενο της `index.php`:

```php
<?php
/* Εκκίνηση της μηχανής του Wordpress: */
define('WP_USE_THEMES', false);
require('../wp-blog-header.php');

/* Εμφάνιση της κεφαλίδας: */
get_header(); ?>

/* Κυρίως περιεχόμενο της σελίδας: */
 <section id="content">

<?php
if ( is_user_logged_in() ) {
    echo '<h1>Welcome, registered user!</h1>';
} else {
    echo '<h1>Welcome, visitor!</h1>';
};
?>

</section>

/* Εμφάνιση του υποσέλιδου: */
<?php get_footer(); ?>;
```

Στη συνέχεια, δημιούργησα επιπλέον χρήστες στο WordPress, δίνοντάς τους 
τον ρόλο <strong>Subscriber</strong>. Οι χρήστες με αυτόν τον ρόλο 
μπορούν μόνο να διαβάσουν δημοσιεύσεις στο ιστολόγιο, δεν έχουν άλλα 
δικαιώματα, όμως από τη μηχανή που κινεί το WordPress θεωρούνται 
<em>συνδεδεμένοι</em>. 

Το αποτέλεσμα: μπορώ να φτιάχνω λογαριασμούς για χρήστες στον 
εξυπηρετητή μου, και να τους εμφανίζω ό,τι περιεχόμενο θέλω εγώ (εκτός 
του ιστολογίου πάντα), το οποίο είναι κρυφό για οποιονδήποτε μη 
συνδεδεμένο επισκέπτη. Αυτό για να γίνει με πρωτότυπο κώδικα php, θέλει 
ώρες προγραμματισμού, χώρια οι γνώσεις που χρειάζονται. Γι' αυτό, το 
WordPress δεν παίζεται!

Προτεινόμενο διάβασμα σχετικά με το θέμα:
<ul>
  <li><a href="http://codex.wordpress.org/Integrating_WordPress_with_Your_Website">Integrating WordPress with Your Website</a></li>
  <li><a href="http://codex.wordpress.org/Roles_and_Capabilities">WordPress user roles and capabilities</a></li>
  <li><a href="http://www.wphardcore.com/2010/ultimate-guide-to-roles-and-capabilities/">Ultimate guide to roles and capabilities</a></li>
</ul>
Και ένα ακόμα μεζεδάκι: αν έχετε ορίσει προσαρμοσμένους <strong>μόνιμους συνδέσμους</strong> στο ιστολόγιό σας (permalinks) τότε το WordPress θα έχει δημιουργήσει στον ριζικό φάκελο της εγκατάστασης ένα κρυφό αρχείο <strong>.htaccess</strong>. Το αποτέλεσμα είναι ότι αν φτιάξετε δικούς σας υποφακέλους μέσα στο φάκελο που είναι εγκατεστημένο το WordPress, και προσπαθήσετε να τους δείτε γράφοντας τη διαδρομή τους στη μπάρα διεύθυνσης του περιηγητή ιστού, πιθανότατα θα πάρετε ένα ωραιότατο σφάλμα 404. 

Για να το αποφύγετε αυτό, πρέπει να ορίσετε ρητά ότι ο φάκελος που φτιάξατε εξαιρείται από τους μόνιμους συνδέσμους, προσθέτοντας στο αρχείο .htaccess τη γραμμή: <code>RewriteRule ^folder-name$ - [L]</code>, αντικαθιστώντας το «folder-name» με το όνομα του φακέλου που φτιάξατε.

Δείτε επίσης
------------

Δύο ακόμα άρθρα σε αυτή τη σειρά:

1.  [Γιατί το WordPress δεν παίζεται #1](/posts/γιατί-το-wordpress-δεν-παίζεται-1/)
2.  [Γιατί το WordPress δεν παίζεται #2](/posts/γιατί-το-wordpress-δεν-παίζεται-2/)
