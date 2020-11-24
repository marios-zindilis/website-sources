---
title: How to get Google search suggestions in Firefox on Linux Mint
description: How to get Google search suggestions in Firefox on Linux Mint
first-published: 2014-03-29
---

The Google search feature is not offered as an option in the version 
of Firefox that comes 
installed in Linux Mint. You can add it with the **Manage Search 
Engines** feature, but when you do, you will notice that there are no 
search suggestions when you start typing a search term.

To add Google search suggestions, you need to edit the file 
`google.xml`, located in the `searchplugins` directory, inside your 
Firefox user profile. In my case, the full path for this file is:

    /home/marios/.mozilla/firefox/mwad0hks.default/searchplugins/google.xml

Yours should be similar, and the command `locate google.xml` can help 
you find the path. 

Edit that file, and add the following line:

    <Url type="application/x-suggestions+json" method="GET" template="https://www.google.com/complete/search?client=firefox&amp;q={searchTerms}"/>

...somewhere before the closing `</SearchPlugin>` tag. Restart your 
Firefox, and suggestions should now work.
