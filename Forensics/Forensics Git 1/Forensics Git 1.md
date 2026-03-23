# Forensics Git 1
Number of Points: 300

## Description
Can you find the flag in this disk image? Download the disk image here.

## Hints
* How can you checkout the files of a previous commit?

## Analysis & Solution
I think picoCTF was being generous for giving 300 points for this one!

We follow the same steps as we did in [Forensics Git 0](../Forensics%20Git%200/Forensics%20Git%200.md).
This time there is no `note.txt`. (Not that it matters at all.)

Let us look at git commit history.
```bash
# git log
commit 5fb8194539c770a830b8ba089a50778c07072b03 (HEAD -> master)
Author: ctf-player <ctf-player@example.com>
Date:   Wed Nov 19 09:20:05 2025 +0000

    Remove flag

commit 177789af0b300e043ea8f54ea57d6cee352291ae
Author: ctf-player <ctf-player@example.com>
Date:   Wed Nov 19 09:20:05 2025 +0000

    Add flag
```
So it seems that flag was added, then removed.
By checking out the commit `177789af0b300e043ea8f54ea57d6cee352291ae`, we may be able to extract the flag.

This is as easy as
```bash
# Checking out the commit
git checkout 177789af0b300e043ea8f54ea57d6cee352291ae

# There is now a file called flag.txt
cat flag.txt
```
which should reveal the flag.
