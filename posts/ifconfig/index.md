---
language: el
title: ifconfig
first-published: 2012-05-10
description: Σημειώσεις για το εργαλείο ifconfig
---

Το εργαλείο γραμμής εντολών ifconfig μπορεί να εμφανίσει πληροφορίες 
σχετικά με τις διεπαφές δικτύου του συστήματος, και να κάνει ρυθμίσεις 
σε αυτές. Για παράδειγμα, μπορείτε να το χρησιμοποιήσετε για να δείτε 
την διεύθυνση MAC μιας διεπαφής, ή αν υπάρχουν συγκρούσεις πακέτων 
μεταξύ του συστήματος και της επόμενης συσκευής δικτύου (collissions).

Αν χρησιμοποιηθεί χωρίς καμμιά παράμετρο, απλά εμφανίζει όλες τις 
διεπαφές δικτύου και μερικές πληροφορίες για αυτές. Για να δείτε 
πληροφορίες για μια μόνο διεπαφή, δώστε το όνομά της μετά την εντολή, 
δηλαδή:

```bash
ifconfig eth0
```

Ενεργοποίηση μιας διεπαφής και ανάθεση της διεύθυνσης δικτύου 192.168.2.3/24:

```bash
ifconfig eth0 up 192.168.2.3 netmask 255.255.255.0
```
