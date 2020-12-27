---
layout: page
editor: Marios Zindilis
license: CC-BY-SA-3.0
title: Preliminaries
first-published: 2016-11-13
description: Some basics before starting ad hoc data analysis with command line tools
---

<p class="lead">
Here's some preliminaries worth noting.
</p>

## Formatting ##

These typesetting conventions will be used when presenting example interactions 
at the command line:

You type:

```bash
$ command argument1 argument2 argument3
```

You get:

    output line 1 
    output line 2 
    output line 3 
    [...]

The `$` is the shell prompt. What you type is shown in the **You type** section
and command output is shown in the **You get** section.

## Example data ##

I will use the following sample files in the examples.

### The Unix password file ###

The password file can be found at `/etc/passwd`. Every user on the system has 
one line (record) in the file. Each record has six fields separated by colon 
(`:`) characters. The fields are username, encrypted password, userid, default 
group, home directory and default shell. We can look at the first few lines 
with the `head` command, which prints just the first few lines of a file. 
Correspondingly, the `tail` command prints just the last few lines.

You type:
```bash
$ head -5 /etc/passwd 
```

You get:

    root:x:0:0:root:/:/bin/bash 
    bin:x:1:1:bin:/bin:/sbin/nologin 
    daemon:x:2:2:daemon:/sbin:/sbin/nologin 
    adm:x:3:4:adm:/var/adm:/sbin/nologin 
    lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin

### Census data ###

The US Census releases Public Use Microdata Samples (PUMS) on [its website][1]. 
We will use the 1% sample of Washington state's data, the `file pums_53.dat`, 
which can be downloaded [here][2].

  [1]: http://www.census.gov/ "US Census"
  [2]: http://www2.census.gov/census_2000/datasets/PUMS/OnePercent/Washington/

You type:
```bash
$ head -2 pums_53.dat 
```

You get:

    H000011715349 53010 99979997 70 15872 639800 120020103814700280300000300409 
    02040201010103020 0 0 014000000100001000 0100650020 0 0 0 0 0000 0 0 0 0 0 
    05000000000004400000000010 76703521100000002640000000000
    P00001170100001401000010420010110000010147030400100012005003202200000 005301000
    000300530 53079 53 7602 76002020202020202200000400000000000000010005 30 53010
    70 9997 99970101006100200000001047904431M 701049-20116010 520460000000001800000
    00000000000000000000000000000000000000001800000018000208

**Important note**: The format of this data file is described in an excel 
spreadsheet that can be downloaded [here][3].

  [3]: http://ftp2.census.gov/census_2000/datasets/PUMS/FivePercent/5%25_PUMS_record_layout.xls

## Developer efficiency vs. computer efficiency ##

The techniques discussed here are usually extremely efficient in terms of 
developer time, but generally less efficient in terms of compute resources 
(CPU, I/O, memory). This kind of brute force and ignorance may be inelegant, 
but when you don't yet understand the scope of your problem, it is usually best 
to spend 30 seconds writing a program that will run for 3 hours than vice 
versa.

## The online manual ##

The `man` command displays information about a given command (colloquially 
referred to as the command's "man page"). The online man pages are an extremely 
valuable resource; if you do any serious work with the commands presented here, 
you'll eventually read all their man pages top to bottom. In Unix literature 
the man page for a command (or function, or file) is typically referred to as 
`command(n)`. The number `n` specifies a section of the manual to disambiguate 
entries which exist in multiple sections. So, `passwd(1)` is the man page for 
the `passwd` *command*, and `passwd(5)` is the man page for the `passwd` 
*file*. On a Linux system you ask for a certain section of the manual by giving 
the section number as the first argument as in `man 5 passwd`. Here's what the 
`man` command has to say about itself:

You type:
```bash
$ man man 
```

You get:

    man(1)                                                        man(1) 
    NAME
           man - format and display the on-line manual pages 
           manpath - determine user's search path for man pages 

    SYNOPSIS 
           man [-acdfFhkKtwW] [--path] [-m system] [-p string] [-C 
           config_file] [-M pathlist] [-P pager] [-S section_list] 
           [section] name ... 

    DESCRIPTION 
           man formats and displays the on-line manual pages. If you 
           specify section, man only looks in that section of the 
           manual. name is normally the name of the manual page, 
           which is typically the name of a command, function, or 
           file. [...]

Next: [Standard Input, Standard Output, Redirection and Pipes][next]

  [next]: /books/ad-hoc-data-analysis-from-the-unix-command-line/standard-input-standard-output-redirection-and-pipes/
