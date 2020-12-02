---
editor: Marios Zindilis
license: CC-BY-SA-3.0
title: Picking The Data Apart With cut
first-published: 2016-11-13
---

## Fixed width data ##

How many households had just 1 person? Referring to the file layout, we see 
that the 106th and 107th characters of a household record indicate the number 
of people in the household. We can use the `cut` command to pull out just that 
bit of data from each record. The argument `-c106-107` instructs `cut` to print 
the 106th through 107th characters of each line. The `head` command prints just 
the first few lines of a file (or its standard input).

```bash
$ census_data>grep "^H" pums_53.dat  | cut -c106-107 | head -5
03 
02 
03 
02 
02
```

You can give `cut` a comma separated list to pull out multiple ranges. To see 
the household id along with the number of occupants of the household:

```bash
$ census_data>grep "^H" pums_53.dat  | cut -c2-8,106-107 | head -5
000011703 
000024602 
000231203 
000242102 
000250202
```

The `-c` argument is used for working with so called "fixed-width" data. Data 
where the columns of a record are found at certain offset in bytes from the 
beginning of a record. Fixed width data abounds on a Unix system. `ls -l` 
writes its output in a fixed width format:

```bash
$ ls -l /etc | head -5
total 6548 
-rw-r--r--    1 root     root          46 Dec  4 12:23 adjtime 
drwxr-xr-x    4 root     root        4096 Oct  8  2003 alchemist 
-rw-r--r--    1 root     root        1048 Aug 31  2001 aliases 
-rw-r--r--    1 root     root       12288 Oct  8  2003 aliases.db
```

As does `ps`:

```bash
$ ps -u'
USER       PID %CPU %MEM   VSZ  RSS TTY      STAT START   TIME COMMAND 
jrauser  26870  0.0  0.1  2576 1388 pts/0    S    09:45   0:00 /bin/bash 
jrauser   8943  0.0  0.0  2820  880 pts/0    R    12:58   0:00 ps -u
```

Returning to the question of how many 1 person households are there in 
Washington:

```bash
$ grep "^H" pums_53.dat  | cut -c106-107 | grep -c 01
7192
```

7,192, or about 28% of households have only one occupant.

## Delimited data ##

In delimited data, elements of a record are separated by a special 
**delimiter** character. In the password file, fields are delimited by colons:

```bash
$ head -5 /etc/passwd
root:x:0:0:root:/:/bin/bash 
bin:x:1:1:bin:/bin:/sbin/nologin 
daemon:x:2:2:daemon:/sbin:/sbin/nologin 
adm:x:3:4:adm:/var/adm:/sbin/nologin 
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
```

The 7th column of the password file is the user's login shell. How many people 
use bash as their shell?

```bash
$ cut -d: -f7 /etc/passwd | grep -c /bin/bash 
170
```

You can give either `-c` or `-f` a comma separated list, so to see a few users 
that use `tcsh` as their shell:

```bash
$ cut -d: -f1,7 /etc/passwd | grep /bin/tcsh | head -5
iglass:/bin/tcsh
svowell:/bin/tcsh
dsedaris:/bin/tcsh
skine:/bin/tcsh
jhitt:/bin/tcsh
```

### Tricky delimiters ###

The space character is a common delimiter. Unfortunately, your shell probably 
discards all extra whitespace on the command line. You can sneak a space 
character past your shell by wrapping it in quotes, like: `cut -d" " -f 5`.

The tab character is another common delimiter. It can be hard to spot, because 
on the screen it just looks like any other white space. The `od` (octal dump) 
command can give you insight into the precise formatting of a file. For 
instance I have a file which maps first names to genders (with 95% 
probability). When casually inspected, it looks like fixed width data:

```bash
$ head -5 gender.txt
AARON           M 
ABBEY           F 
ABBIE           F 
ABBY            F 
ABDUL           M
```

But on closer inspection there are tab characters delimiting the columns:

```bash
$ od -bc gender.txt | head
0000000 101 101 122 117 116 040 040 040 040 040 040 011 115 012 101 102 
          A   A   R   O   N                          \t   M   \n  A   B 
0000020 102 105 131 040 040 040 040 040 040 011 106 012 101 102 102 111 
          B   E   Y                          \t   F  \n   A   B   B   I 
0000040 105 040 040 040 040 040 040 011 106 012 101 102 102 131 040 040 
          E                          \t   F  \n   A   B   B   Y 
0000060 040 040 040 040 040 011 106 012 101 102 104 125 114 040 040 040 
                             \t   F  \n   A   B   D   U   L 
0000100 040 040 040 011 115 012 101 102 105 040 040 040 040 040 040 040 
                     \t   M  \n   A   B   E
```

The first thing to do is read your system's manpage on `cut`: it may already 
delimit by tab by default. If not, it requires a bit of trickery to get a tab 
character past your shell to the `cut` command. First, many shells have a 
feature called tab completion; when you hit tab they don't actually insert a 
tab, instead they attempt to figure out which file, directory or command you 
want and type that instead. In many shells you can overcome this special 
functionality by typing a `control-v` first. Whatever character you type after 
the `control-v` is literally inserted. Like a space character, you need to 
protect the tab character with quotes or the shell will discard it like any 
other white space separating pieces of the command line.

So to get the ratio of male first names to female first names I might run the 
following commands. Between the double quotes I typed `control-v` and then hit 
tab.

```bash
$ wc -l gender.txt
5017 gender.txt 
$ cut -d" " -f2 gender.txt | grep M | wc -l 
1051 
$ cut -d" " -f2 gender.txt | grep F | wc -l 
3966
```

Apparently there's much more variation in female names than male names.

If your system's `cut` command delimits on tab, the above command becomes 
simply `cut -f2 gender.txt`.

