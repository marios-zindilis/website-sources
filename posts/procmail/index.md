---
title: procmail
description: Notes on procmail mail filter
first-published: 2013-12-31
---

Procmail Recipes Syntax
-----------------------

Very briefly:

*   A line starting with `:0` denotes the beginning of a new recipe.
*   A line starting with `*` denotes the definition of a *condition* 
    inside a recipe.
*   The last line of a recipe is the *action*. Action lines can begin 
    with:

    *   `/` in which case the email is appended to a text file, 
        mbox-style.
    *   `|` in which case the email is piped to a script for 
        further handling.
    *   `! user@example.com` in which case the email is forwarded 
        to the email address defined after the `!`.
    *   `{` which denotes the beginning of a nested recipe.
