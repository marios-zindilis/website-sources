---
first-published: 2016-11-13
editor: Marios Zindilis
license: CC-BY-SA-3.0
title: Standard Input, Standard Output, Redirection and Pipes
---

> "This is the Unix philosophy: Write programs that do one thing and do it 
> well. Write programs to work together. Write programs to handle text streams, 
> because that is a universal interface." 
> -- Doug McIlroy, the inventor of Unix pipes

The commands I'm going to talk about here are called filters. Data passes 
through them and they modify it a bit on the way. These commands read data from 
their **standard input** and write data to their **standard output**. By 
default, standard input is your keyboard and standard output is your screen. 
For example, the `tr` command is a filter that translates one set of characters 
to another. This invocation of `tr` turns all lower case characters to upper 
case characters:

You type:
```bash
$ tr "[:lower:]" "[:upper:]"
hello
```

You get:

    HELLO

Then type:
```bash
i feel like shouting
```

You get:

    I FEEL LIKE SHOUTING

You can exit from this with `[ctrl-d]`. This tells the command line that you're 
done entering input.

You can tell your shell to connect standard output to a file instead of your 
screen using the `>` operator. The term for this is **redirection**. One would 
talk about **redirecting** the output of `tr` to a file. Later you can use the 
`cat` command to write the file to your screen.

You type:
```bash
$ tr a-z A-Z > tr_output
this is a test
[ctrl-d]
$ cat tr_output
```

You get:

    THIS IS A TEST

Many Unix commands that take a file as an argument will read from standard 
input if not given a file. For example, the `grep` command searches a file for 
a string and prints the lines that match. If I wanted to find my entry in the 
password file I might say:

```bash
$ grep jrauser /etc/passwd
jrauser:x:7777:100:John Rauser:/home/jrauser:/bin/bash
```

But I could also redirect a file to `grep`'s standard input using the `<` 
operator. You can see that the `<` and `>` operators are like little arrows 
that indicate the flow of data.

```bash
$ grep jrauser < /etc/passwd
jrauser:x:7777:100:John Rauser:/home/jrauser:/bin/bash
```

You can use the pipe `|` operator to connect the standard output of one command 
to the standard input of the next. The `cat` command reads a file and writes it 
to its standard output, so yet another way to find my entry in the password 
file is:

```bash
$ cat /etc/passwd | grep jrauser
jrauser:x:7777:100:John Rauser:/home/jrauser:/bin/bash
```

For a slightly more interesting example, the `mail` command will send a message 
it reads from standard input. Let's send my entry in the password file to me in 
an email.

```bash
$ cat /etc/passwd | grep jrauser | mail -s "passwd entry" jrauser@example.com
```

## Using output with headers ##

In many situations, you end up with output that has a first line that is a 
header describing the data, and subsequent lines that are the data. An example 
is `ps`:

```bash
$ ps | head -5
  PID TTY           TIME CMD
22313 ttys000    0:00.86 -bash
31537 ttys001    0:00.06 -bash
22341 ttys002    0:00.28 -bash
70093 ttys002    0:00.00 head -5
```

If you wish to manipulate the data but not the header use `tail` with the `-n` 
switch to start at line 2. For example:

```bash
$ ps | tail -n +2 | grep bash | head -5
22313 ttys000    0:00.86 -bash
31537 ttys001    0:00.06 -bash
22341 ttys002    0:00.28 -bash
70120 ttys002    0:00.00 -bash
```

This output shows only `bash` processes because of `grep`.

