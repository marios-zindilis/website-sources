---
title: Regular expressions in python-markdown2 (part 2)
description: Study of the regular expressions used in python-markdown2 library
first-published: 2015-11-22
last-modified: 2016-05-15
tags:
- Python
- Markdown2
- Regular Expressions
- Performance Optimization
---

This article is a look into the performance of one of the
regular expressions used in the
<a href="https://github.com/trentm/python-markdown2" 
title="python-markdown2 on Github"> python-markdown2</a> Python module for
converting Markdown syntax to HTML. It was initially written for pure fun, and
in celebration of its own pointlessness, but eventually the changes proposed
here made it upstream in
<a href="https://github.com/trentm/python-markdown2/pull/207" 
title="python-markdown2 pull request 207">pull request 207</a>.

<!-- read more -->

Replace tabs with spaces
------------------------

This snippet of code replaces tab characters with a predefined number of 
spaces. It is a Python port of the Perl code mentioned by 
[Bart Lateur][bart-lateur] in a post about 
[turning tabs to spaces in Perl][perl-tabs-to-spaces].

### A detour to Perl ###

The initial post in that thread was replacing tabs like this:

```perl
#!/usr/bin/perl -pi
s/\t/    /;
```

That code misses one point: if there is any string before a tab, it will simply 
add four spaces after that string. However, that is not how tabs work. What 
should happen is that enough spaces should be added, until the length of the 
initial string plus the newly added spaces, add up to the next multiple of 
four. So, the suggested substitution in Perl becomes:

```perl
s/(.*?)\t/$1.(' ' x (4-length($1)%4))/ge;
```

There are two flags used there: `g` applies the substitution for all matches of 
the left pattern (`(.*?)\t`). Without that flag, only the first match would be 
processed. The second flag, `e`, forces the substitute 
(`$1.(' ' x (4-length($1)%4))`) to be evaluated as an expression itself. 
Without this flag, the second part would be handled as a raw string.

### Back to Python ###

Here is the Python code, cleaned up a little:

```python
import re
DEFAULT_TAB_LENGTH = 4

_detab_re = re.compile(r'(.*?)\t', re.M)
def _detab_sub(match):
    g1 = match.group(1)
    return g1 + (' ' * (DEFAULT_TAB_LENGTH - len(g1) % DEFAULT_TAB_LENGTH))

def _detab(text):
    if '\t' not in text:
        return text
    return _detab_re.subn(_detab_sub, text)[0]
```

#### Explanation

The `_detab_re` object is a compiled Regular Expression object, built with the 
same pattern as the one used in the Perl example, and with the multiline flag 
enabled (`re.M`). You can test this out at [RegExr][regexr]. The `subn()` 
method of that object is called in the last line. It takes two parameters: the 
`_detab_sub()` function, and the text to be processed. For every match of the 
pattern, `_detab_sub()` is called, and the matched string is passed to the 
`_detab_sub()` function for processing. Finally, `subn()` returns a tuple with 
the text with the pattern substituted, and the number of substitutions that
happened. From that result, only the text is kept, with that `subn()[0]`, 
which seems a bit redundant, since the `sub()` method would do that without 
requiring the `[0]` subscription.

#### No regular expressions please

Here is a Python snippet that does the same thing as the previous one, without 
using regular expressions:

```python
DEFAULT_TAB_LENGTH = 4

def _detab_no_re_sub(l):
    if '\t' not in l:
        return l
    else:
        g1 = l.split('\t', 1)[0]
        output = g1
        output += (' ' * (DEFAULT_TAB_LENGTH - len(g1) % DEFAULT_TAB_LENGTH))
        output += l.split('\t', 1)[1]
        return _detab_no_re_sub(output)

def _detab_no_re(text):
    if not '\t' in text:
        return text
    output = []
    for line in text.splitlines():
        output.append(_detab_no_re_sub(line))
    return '\n'.join(output)
```

#### Performance

In the [previous article on regular expressions in python-markdown2][prev] I 
dismissed the difference between a substring substitution with `re.sub()` 
versus `str.replace()` as being negligible, but in this case it seems that it 
is more substantial. This simple example already indicates some difference:

```python
text = '''
We are
        NOT
in Kansas
        any more!
'''

%timeit _detab(text)
100000 loops, best of 3: 6.14 us per loop

%timeit _detab_no_re(text)
100000 loops, best of 3: 3.82 us per loop
```

To test a larger example, I took [this version of the source code of bzip2][bz]
which uses three spaces for indentation, and made some substitutions in it:

```bash
# Change some spaces in the beginning of lines with tabs:
sed -i 's/^   /\t/' bzip2.c 
sed -i 's/^\t   /\t\t/' bzip2.c
# Lines with tabs:
grep -c '\t' bzip2.c 
3032
# Total lines:
wc -l bzip2.c 
6998 bzip2.c
```

Timing test with this file:

```python
text = file('bzip2.c').read()

%timeit _detab(text)
10 loops, best of 3: 90.1 ms per loop

%timeit _detab_no_re(text)
100 loops, best of 3: 11 ms per loop
```

That is significant difference, not using regular expressions makes the 
process about 8 times faster.

### Conclusion

Based on this article, and on the previous one, I would prefer to use other 
methods for substring replacements than regular expressions.

<!-- Links -->
[python-markdown2]: https://github.com/trentm/python-markdown2 "python-markdown2 on Github"
[perl-tabs-to-spaces]: http://www.nntp.perl.org/group/perl.macperl.anyperl/2002/03/msg154.html "Perl Tabs to Spaces"
[bart-lateur]: http://search.cpan.org/~bartl/ "Bart Lateur"
[regexr]: http://regexr.com/3c8o7 "Tabs to spaces regular expression"
[prev]: /blog/2015/11/07/regex-in-python-markdown2-pt1.html "Regular expressions in python-markdown2 (part 1)"
[bz]: /static/etc/bzip2.c "bzip2.c Source Code"
