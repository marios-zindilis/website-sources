---
first-published: 2016-11-13
editor: Marios Zindilis
license: CC-BY-SA-3.0
title: Joining The Data with join
---

**Please note**: `join` assumes that that input data is sorted based on the key 
on which the join is going to take place.

## Delimited data ##

In delimited data, elements of a record are separated by a special 'delimiter' 
character. In the CSV files, fields are delimited by commas or tabs:

```bash
$ cat j1
1,a
1,b
2,c
2,d
2,e
3,f
3,g
4,h
4,i
5,j
```

```bash
$ cat j2
1,A
1,B
1,C
2,D
2,E
4,F
4,G
5,H
6,I
6,J
```

```bash
$ join -t , -a 1 -a 2 -o 0,1.2,2.2 j1 j2
1,a,A
1,a,B
1,a,C
1,b,A
1,b,B
1,b,C
2,c,D
2,c,E
2,d,D
2,d,E
2,e,D
2,e,E
3,f,
3,g,
4,h,F
4,h,G
4,i,F
4,i,G
5,j,H
6,,I
6,,J
```

Explanation of options:

    "-t ,"          Input and output field separator is "," (for CSV)
    "-a 1"          Output a line for every line of j1 not matched in j2
    "-a 2"          Output a line for every line of j2 not matched in j1
    "-o 0,1.2,2.2"  Output field format specification

For the last option, `0` denotes the match (join) field (needed when using 
`-a`), `1.2` denotes field 2 from file 1 ("j1") and `2.2` denotes field 2 from 
file 2 ("j2").

Using the `-a` option creates a full outer join as in SQL.

This command must be given two and only two input files.

## Multi-file Joins ##

To join several files you can loop through them.

```bash
$ join -t , -a 1 -a 2 -o 0,1.2,2.2 j1 j2 > J
```


File "J" is now the full outer join of "j1", "j2".

```bash
$ join -t , -a 1 -a 2 -o 0,1.2,2.2 J j3 > J
```

and so on through j4, j5&hellip;

For many files this is best done with a loop:

```bash
$ for i in * ; do join -t , -a 1 -a 2 -o 0,1.2,2.2 J $i > J ; done
```

## Sorted Data Note ##

`join` assumes that the input data has been sorted by the field to be joined. 
See section on `sort` for details.

