---
title: Git, symlinks και hardlinks
Author: Marios Zindilis
first-published: 2011-04-23
description: Σημειώσεις για την συμπεριφορά του Git σε σχέση με τα symlinks και τα hardlinks
---

<strong>Γρήγορη συμβουλή για το Git</strong>: Ήθελα να συμπεριλάβω σε ένα 
αποθετήριο Git, ένα αρχείο το οποίο τραβούσε το πρόγραμμα από άλλο φάκελο, 
εκτός του τοπικού αποθετηρίου. Είδα ότι με δημιουργία συμβολικού δεσμού το Git 
ανέβαζε στο απομακρυσμένο αποθετήριο το αρχείο σαν απλό αρχείο κειμένου, με 
περιεχόμενο τη διαδρομή του συνδεδεμένου αρχείου. Με λίγο ψάξιμο βρήκα ότι δεν 
υπάρχει τρόπος να ακολουθεί το Git τους συμβολικούς δεσμούς, για μερικούς 
καλούς λόγους, ένας από τους οποίους είναι ότι δημιουργείται ένα τεράστιο κενό 
ασφαλείας, σε περίπτωση που γίνει check-out σε ένα σύστημα στο οποίο το 
αρχείο-στόχος του συμβολικού δεσμού μπορεί να έχει εντελώς διαφορετικό 
περιεχόμενο!

Ωστόσο, μπορείτε να χρησιμοποιήσετε hardlinks, δημιουργώντας τα με: <code>ln /absolute/path/to/destination link-path</code>, με μερικά όμως μειονεκτήματα, ένα από τα οποία είναι ότι κατά το check-out το αρχείο δημιουργείται ως κανονικό αρχείο στο τοπικό αποθετήριο όπου κάνετε check-out και χάνεται η ιδιότητά του ως δεσμός. Αυτό βέβαια λύνει και το πρόβλημα ασφαλείας που ανέφερα πριν.

Μια ακόμα σημείωση, μέχρι πριν την έκδοση 1.6.1, το Git ακολουθούσε τους συμβολικούς δεσμούς, αν αυτοί ήταν φάκελοι αντί για αρχεία. Αυτό διορθώθηκε, και στις πιο πρόσφατες εκδόσεις δεν δουλεύει.
