---
first-published: 2016-11-13
editor: Marios Zindilis
license: CC-BY-SA-3.0
title: Rewriting The Data With Inline perl
---

> "I'm reminded of the day my daughter came in, looked over my shoulder at some 
> Perl 4 code, and said, 'What is that, swearing?'"
> -- Larry Wall

## Command Line perl ##

A tutorial on perl is beyond the scope of this document; if you don't know 
perl, you should learn at least a little bit. If you invoke perl like 
`perl -n -e '#a perl statement'` the `-n` option causes perl to wrap your `-e` 
argument in a implicit `while` loop like this:

```perl
while (<>) {
   # a perl statement
}
```

This loop reads standard input a line at a time into the variable `$_`, and 
then executes the statement(s) give by the `-e` argument. Given `-p` instead of 
`-n`, perl to adds a `print` statement to the loop as well:

```perl
while (<>) {
   # a perl statement
   print $_;
}
```

## Example - Using perl to create an indicator variable ##

Education level is recorded in columns 53-54 as ordered set of categories, 
where 11 and above indicates a college degree. Let's condense this to a single 
indicator variable for completed college or not. The raw data:

```bash
$ cat pums_53.dat | grep "^P" | cut -c53-54 | head -5
12
11
06
03
08
```

And once passed through the perl script:

```bash
$ cat pums_53.dat | grep "^P" | cut -c53-54 | 
perl -ne 'print $_>=11?1:0,"\n"' | head -5
1
1
0
0
0
```

And the final result:

```bash
~/census_data>cat pums_53.dat | grep "^P" | cut -c53-54 |
perl -ne 'print $_>=11?1:0,"\n"' | sort | uniq -c
37507 0
21643 1
```

About 36% of Washingtonians have a college degree.

## Example - computing conditional probability of membership in two sets ##

Let's look at the relationship between education level and whether or not 
people ride their bikes to work. People's mode of transportation to work is 
encoded as a series of categories in columns 191-192, where category 9 
indicates a bicycle. We'll use an inline perl script to rewrite both education 
level and mode of transportation:

```bash
$ cat pums_53.dat | grep "^P" | cut -c53-54,191-192 | 
perl -ne 'print substr($_,0,2)>=11?1:0,substr($_,2,2)==9?1:0,"\n";' | 
sort | uniq -c 
37452 00
   55 01
21532 10
  111 11
```

`55/(55+36447) = 0.15%` of non college educated people ride their bike to work. 
`111/(111+20219) = 0.56%` of college educated people ride their bike to work.

Sociological interpretation is left as an exercise for the reader.

## Example - A histogram with custom bucket size ##

Suppose we wanted to take a look at distribution of personal incomes. The 
normal trick of `sort` and `uniq` would work, but the personal income in the 
census data has resolution down to the $10 level, so the output would be very 
long and it would be hard to quickly see the pattern. We can use perl to round 
the income data down to the nearest $10,000 on the fly. Before the inline perl 
script:

```bash
$ cat pums_53.dat | grep "^P" | cut -c297-303 | head -4
0018000
0004100
0004300
0005300
```

And after:

```bash
$ cat pums_53.dat | grep "^P" | cut -c297-303 | 
perl -pe '$_=10000*int($_/10000)."\n"' | head -4
10000
0
0
0
```

And finally, the distribution (up to $100,000). The extra `grep [0-9]` ensures 
that blank records are not considered in the distribution.

```bash
$ cat pums_53.dat | grep "^P" | cut -c297-303 | grep [0-9] |
perl -pe '$_=10000*int($_/10000)."\n"' | sort -n | uniq -c | head -12
   20 -10000
15193      0
 8038  10000
 6776  20000
 5436  30000
 3685  40000
 2370  50000
 1536  60000
  899  70000
  521  80000
  326  90000
  283 100000
```

## Example - Finding the median (or any percentile) of a distribution ##

If we sort all the incomes in order and had a way to pluck out the middle 
number, we could easily get the median. I'll give two ways to do this. The 
first uses `cat -n`. If given the `-n` option, `cat` prepends line numbers to 
each line. We see that there are 46,359 non blank records, so the 23179th one 
in sorted order is the median.

```bash
$ cat pums_53.dat | grep "^P" | cut -c297-303 | grep [0-9] | wc -l
46359 
$ cat pums_53.dat | grep "^P" | cut -c297-303 | grep [0-9] | sort | 
cat -n | grep "^ *23179"
23179 0019900
```

An even simpler method, using head and tail:

```bash
$ cat pums_53.dat | grep "^P" | cut -c297-303 | grep [0-9] | sort |
head -23179| tail -1
0019900
```

The median income in Washington state in 2000 was $19,900.

## Example - Finding the average of a distribution ##

What about the average? One way to compute the average is to accumulate a 
running sum with perl, and do the division by hand at the end:

```bash
$ cat pums_53.dat | grep "^P" | cut -c297-303 | grep [0-9] | 
perl -ne 'print  $sum+=$_,"\n";' | cat -n | tail -1
46359 1314603988
```

$1314603988/ 46359 = $28357.0393666818

You could also get perl to do this division with an `END` block which perl will 
execute only after it has exhausted standard input:

```bash
$ cat pums_53.dat | grep "^P" | cut -c297-303 | grep [0-9] | 
perl -ne '$sum += $_; $count++; END {print $sum/$count,"\n";}' 
28357.0393666818
```

