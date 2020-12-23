---
title: switchport port-security mac-address sticky
language: el
first-published: 2011-09-29
---

Η εντολή `switchport port-security mac-address sticky` καθορίζει ότι οι 
φυσικές διευθύνσεις MAC που δικαιούνται να επικοινωνούν μέσω της 
συγκεκριμένης διεπαφής, είναι αυτές που είναι ήδη συνδεδεμένες, ή οι 
πρώτες που θα συνδεθούν. Απαιτείται πρώτα να οριστεί η συγκεκριμένη 
διεπαφή ως access: [switchport mode access](/posts/cisco-switchport-mode-access/) 
και να ενεργοποιηθεί σ' αυτήν η ασφάλεια διεπαφής: [switchport port-security](/posts/cisco-switchport-port-security/). 

Δείτε επίσης
------------

*   Εναλλακτικά, μπορείτε να καθορίσετε ρητά ότι η διεπαφή θα επιτρέπει 
    μόνο συγκεκριμένες φυσικές διευθύνσεις MAC που θα ορίσετε, με την 
    εντολή [switchport port-security mac-address $MAC-ADDRESS](/posts/cisco-switchport-port-security-mac-address/).
