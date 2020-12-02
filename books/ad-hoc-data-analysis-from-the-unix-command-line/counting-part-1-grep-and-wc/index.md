---
editor: Marios Zindilis
license: CC-BY-SA-3.0
title: Counting Part 1 - grep and wc
first-published: 2016-11-13
---

> "90% of data analysis is counting" - John Rauser

&hellip;well, at least once you've figured out the right question to ask, which 
is, perhaps, the other 90%.

## Example - Counting the size of a population ##

The simplest command for counting things is `wc`, which stands for 
*word count*. By default, `wc` prints the number of lines, words, and 
characters in a file.

```bash
$ wc pums_53.dat
85025 1219861 25659175 pums_53.dat
```

Nearly always we just want to count the number of lines (records), which can be 
done by giving the `-l` option to `wc`:

```bash
$ wc -l pums_53.dat
85025 pums_53.dat
```

## Example - Using grep to select a subset ##

So, recalling that this is a 1% sample, were there 8.5 million people in 
Washington as of the 2000 census? Nope, the census data has two kinds of 
records, one for households and one for persons. The first character of a 
record, an H or P, indicates which kind of record it is. We can `grep` for and 
count just person records like this:

```bash
$ grep -c "^P" pums_53.dat
59150
```

The caret `^` means that the `P` must occur at the beginning of the line. So 
there were about 5.9 million people in Washington State in 2000. Also 
interesting, the average household had `59,150/(85,025-59,150) = 2.3` people.

