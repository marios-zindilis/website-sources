---
title: "Bash: Check that string is IP"
description: Some non-regex ways to validate an IP in Bash
first-published: 2013-05-10
last-modified: 2015-03-23
---

Assuming that we have a Bash script that accepts a parameter  that is expected to be an IP Address, this article lists
a couple of (practically useless) ways of verifying that input *without using regular expressions*, which is the sane
thing to do. It is therefore an exercise in futility, but it helps with my chronic allergy to regular expressions.

<!-- read more -->

Breakdown of the conditions
---------------------------

First, check that <strong>exactly three dots exist</strong> in the string:
<pre>
if [ `echo $IsIP | grep -o '\.' | wc -l` -ne 3 ]; then
    echo "Parameter '$1' does not look like an IP Address.";
    exit 1;
fi
</pre>

Then, check that <strong>exactly four parts exist</strong> if the string is broken down by dots:
<pre>
if [ `echo $IsIP | tr '.' ' ' | wc -w` -ne 4 ]; then
    echo "Parameter '$1' does not look like an IP Address.";
    exit 1;
fi
</pre>

Then, check that <strong>all four parts are numeric</strong>:
<pre>
for OCTET in `echo $IsIP | tr '.' ' '; do
    if ! [[ $OCTET =~ ^[0-9]+$ ]]; then
        echo "Parameter '$1' does not look like an IP Address.";
        exit 1;
    fi
done
</pre>

Finally check that <strong>all four parts are in the range 0 to 255</strong>:
<pre>
for OCTET in `echo $IsIP | tr '.' ' '; do
    if [[ $OCTET -lt 0 || $OCTET -gt 255 ]]; then
        echo "Parameter '$IsIP' does not look like in IP Address (octet '$OCTET' in not in range 0-255).";
    fi
done
</pre>

So the parameter passed to the script must fulfill the following four criteria:
<ol>
<li>Contain exactly three dots.</li>
<li>Contain exactly four parts, if broken down by dots.</li>
<li>All four parts are numeric.</li>
<li>All four parts are between 0 and 255.</li></ol>

<h2>A Bash function</h2>
Here's the entire thing in the form of a Bash function, with an additional comment on what went wrong:
<pre>
# Verify that the parameter passed is an IP Address:
function is_IP() {
if [ `echo $1 | grep -o '\.' | wc -l` -ne 3 ]; then
        echo "Parameter '$1' does not look like an IP Address (does not contain 3 dots).";
        exit 1;
elif [ `echo $1 | tr '.' ' ' | wc -w` -ne 4 ]; then
        echo "Parameter '$1' does not look like an IP Address (does not contain 4 octets).";
        exit 1;
else
        for OCTET in `echo $1 | tr '.' ' '`; do
                if ! [[ $OCTET =~ ^[0-9]+$ ]]; then
                        echo "Parameter '$1' does not look like in IP Address (octet '$OCTET' is not numeric).";
                        exit 1;
                elif [[ $OCTET -lt 0 || $OCTET -gt 255 ]]; then
                        echo "Parameter '$1' does not look like in IP Address (octet '$OCTET' in not in range 0-255).";
                        exit 1;
                fi
        done
fi

return 0;
}
</pre>

Example output of the function when saved as a standalone script:
<pre>
marios@yovan ~ $ ./is_IP 1234
Parameter '1234' does not look like an IP Address (does not contain 3 dots).
marios@yovan ~ $ ./is_IP 1234...
Parameter '1234...' does not look like an IP Address (does not contain 4 octets).
marios@yovan ~ $ ./is_IP 1.2.3.A
Parameter '1.2.3.A' does not look like in IP Address (octet 'A' is not numeric).
marios@yovan ~ $ ./is_IP 1.2.3.300
Parameter '1.2.3.300' does not look like in IP Address (octet '300' in not in range 0-255).
marios@yovan ~ $ ./is_IP 1.2.3.4
marios@yovan ~ $
</pre>

If someone has any better ways to check this, or improvements on the above, please step forward :)

<a href="https://github.com/marios-zindilis/Scripts/tree/master/Bash" title="Fork this script on GitHub.">Fork these scripts on GitHub.</a>

Using Regular Expressions
-------------------------

In an older blog platform that I used, [Efstathios Chatzikyriakidis](http://efxa.org/) 
commented and suggested the use of regular expressions, which in 
retrospect made all the above pretty useless. The expression that I 
found working best is the one at [IP Address regular expression](http://www.geekzilla.co.uk/View0CBFD9A7-621D-4B0C-9554-91FD48AADC77.htm).
