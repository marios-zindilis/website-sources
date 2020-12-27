---
editor: Marios Zindilis
license: CC-BY-SA-3.0
title: Counting Part 2 - sort and uniq
first-published: 2016-11-13
description: Counting string occurrences with sort and uniq
---

So far we've seen how to use `cut`, `grep` and `wc` to select and count records 
with certain qualities. But each set of records we'd like to count requires a 
separate command, as with counting the numbers of male and female names in the 
most recent example. Combining the `uniq` and `sort` commands allows us to 
count many groups at once.

## uniq and sort ##

The `uniq` command squashes out contiguous duplicate lines. That is, it copies 
from its standard input to its standard output, but if a line is identical to 
the immediately preceding line, the duplicate line is not written. For example:

```bash
$ cat foo
a 
a
a
b
b
a
a
a
c
$ uniq foo
a
b
a
c
```

Note that `a` is written twice because `uniq` compares only to the immediately 
preceding line. If the data is sorted first, we get each distinct record just 
once:

```bash
$ sort foo | uniq
a
b
c
```

Finally, giving the `-c` option causes `uniq` to write counts associated with 
each distinct entry:

```bash
$ sort foo | uniq -c
6 a
2 b
1 c
```

Sorting a CSV file by an arbitrary column is easy as well:

```bash
$ cat file.csv
a, 10, 0.5
b, 20, 0.1
c, 14, 0.01
d, 55, 0.23
e, 94, 0.78
f, 1,  0.34
g, 75, 1.0
h, 3,  2.0
i, 12, 1.5
```
```bash
$ sort -n -t"," -k 2 file.csv
f, 1,  0.34
h, 3,  2.0
a, 10, 0.5
i, 12, 1.5
c, 14, 0.01
b, 20, 0.1
d, 55, 0.23
g, 75, 1.0
e, 94, 0.78
```
```bash
$ sort -n -t"," -k 3 file.csv
c, 14, 0.01
b, 20, 0.1
d, 55, 0.23
f, 1,  0.34
a, 10, 0.5
e, 94, 0.78
g, 75, 1.0
i, 12, 1.5
h, 3,  2.0
```

## Example - Creating a histogram ##

The combination of sort and `uniq -c` is extremely powerful. It allows one to 
create histograms from virtually any record oriented text data. Returning to 
the name-to-gender mapping of the previous chapter, we could have gotten the 
count of male and female names in one command like this:

```bash
$ cut -d" " -f2 gender.txt | sort | uniq -c
3966 F
1051 M
```

## Example - Creating another histogram ##

And returning to the census data, we can now easily compute the complete 
distribution of occupants per household:

```bash
$ grep "^H" pums_53.dat  | cut -c106-107 | sort | uniq -c
1796 00
7192 01
7890 02
3551 03
3195 04
1391 05
 518 06
 190 07
  79 08
  39 09
  14 10
  14 11
   3 12
   3 13
```

## Example - Verifying a primary key ##

This is a good opportunity to point out a big benefit of being able to play 
with data in this fashion. It allows you to quickly spot potential problems in 
a dataset. In the above example, why are there 1,796 households with 0 
occupants? As another example of quickly verifying the integrity of data, let's 
make sure that household id is truly a unique identifier:

```bash
$ grep "^H" pums_53.dat | cut -c2-8 | sort | uniq -c | grep -v "^ *1 " | wc -l
0
```

This `grep` invocation will print only lines that do not (because of the `-v` 
flag) begin with a series of spaces followed by a `1` (the count from 
`uniq -c`) followed by a tab (entered using the `control-v` trick). Since the 
number of lines written is zero, we know that each household id occurs once and 
only once in the file.

The technique of grepping `uniq`'s output for lines with a certain count is 
generally useful. One other common application is finding the set of 
overlapping (duplicated) keys in a pair of files by grepping the output of 
`uniq -c` for lines that begin with a `2`.

## Example - A histogram sorted by most common category ##

Throwing an extra `sort` on the end of the pipeline will sort the histogram so 
that the most common class is at the top (or bottom). This is useful when data 
is categorical and does not have a natural order. You'll want to give `sort` 
the `-n` option so that it sorts the counts numerically instead of lexically, 
and I like to give the `-r` option to reverse the sort so that the output is 
sorted in descending order, but this just a stylistic issue. For example, here 
is the distribution of household heating fuel from most common to least common:

```bash
$ grep "^H" pums_53.dat | cut -c132 | sort | uniq -c | sort -rn
12074 3
 7007 1
 3161 
 1372 6 
 1281 4 
  757 2
  170 8
   43 9
    6 5
    4 7
```

Type 3, electricity, is most common, followed by type 1, gas. Type 7 is solar 
power.

## Converting the histogram to proper CSV ##

The output of `uniq -c` is not in proper CSV form. This makes is necessary to 
convert the output if further operations on the output are wanted. Here we use 
a bit of **inline perl** to rewrite the lines and reverse the order of the 
fields:

```bash
$ cut -d" " -f2 gender.txt | sort | uniq -c | perl -pe 's/^\s*([0-9]+) (\S+).*/$2, $1/' 
F, 3966
M, 1051
```

