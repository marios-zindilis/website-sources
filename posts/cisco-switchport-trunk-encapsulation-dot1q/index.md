---
title: switchport trunk encapsulation dot1q
language: el
description: Σημειώσεις για την εντολή switchport trunk encapsulation dot1q σε συσκευές Cisco
first-published: 2011-09-29
---

Η εντολή `switchport trunk encapsulation dot1q` καθορίζει το 
encapsulation στο οποίο θα λειτουργεί μια διεπαφή, η οποία ήδη 
λειτουργεί ως trunk. Η παράμετρος `dot1q` καθορίζει την τιμή στο 
πρότυπο IEEE 802.1q.

Υπάρχουν δύο πρότυπα λειτουργίας για διεπαφές trunk, το 802.1q και το ISL. Το πρότυπο ISL (Inter-Switch Link) αναπτύχθηκε από τη Cisco πολύ νωρίτερα από το 802.1q. Μια βασική του διαφορά από το 802.1q είναι ότι περικλείει το Ethernet Frame σε ένα επιπλέον ζεύγος header και trailer. Αντίθετα, το 802.1q προσθέτει μόνο ένα επιπλέον πεδίο στο υφιστάμενο header του Ethernet frame, και αναγκαστικά επανυπολογίζει το Frame Check Sequence (FCS) στο trailer.

Πρακτικά, το ISL δεν χρησιμοποιείται πλέον σήμερα, και σε νεότερες συσκευές η ίδια η Cisco έχει σταματήσει να το υποστηρίζει. Αναφέρεται ακόμα στην τεκμηρίωση για σκοπούς συμβατότητας με παλαιότερες συσκευές. Οι συσκευές που δεν προσφέρουν άλλη δυνατότητα ρύθμισης, λειτουργούν από προεπιλογή σε dot1q. 

Δείτε επίσης
------------

Η εντολή `switchport trunk encapsulation` μπορεί να πάρει τρεις τιμές. 
Η βασική εναλλακτική του `dot1q` είναι η [switchport trunk encapsulation isl](/posts/cisco-switchport-trunk-encapsulation-isl/), 
και για αυτόματη διαπραγμάτευση του encapsulation μεταξύ των δύο συσκευών δείτε το 
[switchport trunk encapsulation negotiate](/posts/cisco-switchport-trunk-encapsulation-negotiate/). 
