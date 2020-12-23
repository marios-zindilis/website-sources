---
title: less
Description: The less pager
first-published: 2012-02-09
last-modified: 2013-12-24
---

`less`, similarly to `more`, can display a page-by-page view of a text 
document, on the command line. Compared to `more`, `less` has more 
features, the most significant of which is backwards scrolling. In a 
sense, `less` is like a version of **Vi**, but with a restricted 
feature set.

Navigation
----------

You can navigate in a document with `less`:

*   Downwards, one line at a time with **Enter** or **Down** arrow.
*   Upwards, one line at a time with **Up** arrow.
*   Downwards, one page at a time with **Spacebar**, or **Ctrl+V**, or **PageDown**.
*   Upwards, on page at a time with **Esc+V** or **PageUp**.

Searching
---------

You can search forwards in a document in `less` with the `/` key, which 
will take you to the first instance of the search term used, after the 
current line. You can also search backwards in the document, by 
prefixing you search term with `?` instead of `/`.

In both cases, you can jump to the next instance of the search term 
with `n`, and to the previous one with `N`.

Exiting
-------

You can exit `less` with the **q** key, or with **Ctrl+C**.

