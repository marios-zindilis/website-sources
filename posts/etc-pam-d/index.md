---
title: /etc/pam.d/
description: Note on PAM configuration files in /etc/pam.d/
first-published: 2013-12-31
Last Updated: 2014-02-27
---

Files in `/etc/pam.d/` are configurations for <abbr title='Pluggable 
Authentication Modules'>PAM</abbr> stacks. The generic syntax of a line 
in those files is:

    management_group control_flag module [options]

More on `management_group` and `control_flag` further in this article. 
The `module` is the name of the PAM file to be used. The `options` are 
not required, and are either generic ones or module-specific.

`management_group`
------------------

The value of `management_group` can be one of:

*   `auth` (for authentication)
*   `account` (for account management)
*   `session` (for session management)
*   `password` (for password managemet)

### `auth` ###

The `auth` group is used for user authentication, and is mostly used by 
tools like `login` for CLI authentication or XDM or similar for logging 
in to a desktop environment.

### `passwd` ###

The `passwd` group is used for user password management, and is most 
likely utilized by tools like `passwd`.

### `session` ###

The `session` group manages user sessions. It may verify the existence 
of a user's home directory or even create it if it does not exist, it 
can mount partitions that are specific to a user, etc. It will also 
clean up the user's session after he/she has logged out.

`control_flag`
--------------

The value of `control_flag` can be one of:

*   `requisite`
*   `required`
*   `optional`
*   `sufficient`

### `requisite` ###

The `requisite` flag makes a check necessary but not enough. This means 
that a `requisite` check **must** succeed for the stack to go on, but 
the success of the entire stack depends on further checks. In 
pseudocode:

    IF SUCCESS:
        GOTO NEXT LINE
    ELSE:
        FAIL

### `required` ###

The `required` flag makes a check necessary for the success of the 
entire stack, while it allows for the execution of the next checks. In 
pseudocode:

    IF SUCCESS:
        GOTO NEXT LINE
    ELSE:
        GOTO NEXT LINE
    FINALLY:
        FAIL

### `sufficient` ###

The `sufficient` flag makes a check stop the execution of the stack if 
that check succeeds, otherwise execution continues. In pseudocode:

    IF SUCCESS:
        STOP STACK EXECUTION
    ELSE:
        GOTO NEXT LINE

### `optional` ###

The `optional` flag does not affect the execution of the stack, unless 
the check is the last one in the stack, in which case the success of 
the entire stack is the same as the success of the last check. In 
pseudocode:

    IF SUCCESS:
        GOTO NEXT LINE
    ELSE:
        GOTO NEXT LINE

Note that the pseudocode above does not include the exception that 
happens when the `optional` check is that last in the stack.
