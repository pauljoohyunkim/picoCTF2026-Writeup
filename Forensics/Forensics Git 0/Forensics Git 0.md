# Forensics Git 0
Number of Points: 200

## Description
Can you find the flag in this disk image? Download the disk image here.

## Hints
* How can you extract the directory from the disk image?

## Analysis & Solution
While there are a lot of ways to inspect disk image (say, using Sleuthkit, kpartx, etc.)
I found that using 7zip extracts pretty much any archive successfully.

Note that this is not applicable for some disk analysis challenges as it may get rid of "removed files" that should be inspected,
but for most challenges, this is the easiest way to get started.

```bash
# To get started, download and extract.
wget https://challenge-files.picoctf.net/c_plain_mesa/96db2eea3d6d3e215d3dc2289457a1bc10b17b1de69c46996a171f4f689db74b/disk.img.gz
gzip -d disk.img.gz
7z x disk.img
# There are now 0.img, 1.img, 2.img. These are partitions within the image.
# It turns out 2.img is the only thing that contains the main partition.
7z x 2.img
```

Navigating to `/home/ctf-player`, there is a directory called `Code`.
Inside, there is a directory called `secrets`.

Within this directory, using `ls -la`, we see that there is a file called `note.txt` and a directory called `.git`,
which suggests that this is in fact a git repository.

`note.txt` did not contain anything useful.
```
# cat note.txt
The picoCTF flag format is 'picoCTF{}' where there is some leetspeak phrase in between the curly braces
```

Since this is a git repository, we could inspect the commit history.
```
# git log
commit 327681bb38cf467cec328eec9707b240e3e74ced (HEAD -> master)
Author: ctf-player <ctf-player@example.com>
Date:   Wed Nov 19 08:49:27 2025 +0000

    Wrap this phrase in the flag format: FLAG_HERE_BUT_I_REDACTED_IT
```

This revealed the flag!
