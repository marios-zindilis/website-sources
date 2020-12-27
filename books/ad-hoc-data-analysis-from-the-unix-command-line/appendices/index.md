---
editor: Marios Zindilis
license: CC-BY-SA-3.0
title: Appendices
first-published: 2016-11-13
description: Appendices to the book Ad hoc data analysis from the unix command line
---

## Appendix A: pcalc source code ##

A perl read-eval-print loop. This makes a very handy calculator on the command 
line. Example usage:

```bash
$ pcalc 1+2
3
$ pcalc "2*2"
4
$ pcalc 2*3
6
```

Source:

```perl
#!/opt/third-party/bin/perl
use strict;
if ($#ARGV >= 0) {
  eval_print(join(" ",@ARGV))
} else { 
  use Term::ReadLine;
  my $term = new Term::ReadLine 'pcalc';
  while ( defined ($_ = $term->readline("")) ) {
    s/[\r\n]//g;
    eval_print($_);
    $term->addhistory($_) if /\S/;
  }
}

sub eval_print {
  my ($str) = @_;
  my $result = eval $str;
  if (!defined($result)) {
    print "Error evaluating '$str'\n";
  } else {
    print $result,"\n";
  }
}
```

## Appendix B: Random unfinished ideas ##

Ideas too good to delete, but that aren't fleshed out.

### Micro shell scripts from the command line ###

Example - which .so has the object I want?

### Using backticks ###

#### Example - killing processes by name ####

```bash
kill `ps auxww | grep httpd | grep -v grep | awk '{print $2}'`
```

#### Example - tailing the most recent log file in one easy step ####

```bash
tail -f `ls -rt *log | tail -1`
```

### James' xargs trick ###

James uses `echo` with `xargs` and feeds one `xargs`' output into another 
`xargs` in clever ways to build up complex command lines.

### Also ###

*   `tee(1)`
*   `perl + $/ == agrep`
*   Example - Finding duplicate keys in two files

