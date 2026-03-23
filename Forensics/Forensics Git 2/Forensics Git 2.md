# Forensics Git 2
Number of Points: 400

## Description
The agents interrupted the perpetrator's disk deletion routine. Can you recover this git repo? Download the disk image here.

## Hints
* We think the deletion was interrupted before any git objects were touched

## Analysis & Solution
Now this is more like a nontrivial problem!

Let us do the same process as we did in [Forensics Git 0](../Forensics%20Git%200/Forensics%20Git%200.md) and [Forensics Git 1](../Forensics%20Git%201/Forensics%20Git%201.md).

This time we have `Code/killer-chat-app` directory, but when we do `git log`, it does not work.
It is possible that git directory is corrupted.

However, as the hint suggests, we could still poke around the objects directory.
A quick search on Google suggests that the `.git/objects` directory contains information on the layout of the directories,
commits, etc, so we could try to mine for information there.

After navigating to `.git/objects`, I was not sure how it worked exactly,
```bash
.
├── 01
│   └── 533f718556a0e59f1467dae4fa462eed82c2a1
├── 20
│   └── 1c707b43219a63c1d3499b29c7d539af079861
├── 21
│   └── 51ef0ccc15aed1ab88e1afdc7484aaeff211c4
├── 22
│   └── f7d0c9bd045563ae33bfacfbe46fe406a5b318
├── 26
│   └── b809e0c41d8421f1126ed3a4eb06ad66e6d90a
├── 2c
│   └── 0a9b2b15dce92f800393d5030c7454efc278ae
├── 58
│   └── 27632e046a80a1e0d7b4fc5c7800dd539baeaf
├── 5e
│   └── b896e3ccd51175f66480cdb247fc45f3e8ac2d
├── 66
│   └── 273877d2ff3f51a14473b7200aae5a798ff64f
├── 6b
│   ├── 1ebe10826d5c1efc58ae475c0a0af10f580b77
│   └── f83de540f7d12cc3b683a83d69432e03d84509
├── 71
│   ├── 78644433e7cb6da3adf028f1c80d382a18e7b6
│   └── fd2fafcd5ebd62fbf857769c92a91225ab3954
├── a0
│   └── c13fe974d95661f24e32bc0d79f54f05ea13c5
├── aa
│   └── 1cc01687b4ec94faf9916c3fc6efd83f23b816
├── c9
│   └── 31ae0868411e5f23656a2436e78a4c4699e18c
├── d4
│   └── 666b9472fad7cd75d05b641e402347d9aac605
├── d7
│   └── b4a371ebd23e682ffebc7ec355690fdc94fbd1
├── e8
│   └── 0b38b3322a5ba32ac07076ef5eeb4a59449875
├── ea
│   └── d27e2bd5a0fc22868ffb629a768f82dfcda11c
├── f1
│   └── 50f0b963ab3ee95ba5656212abd76d7f2fed2e
├── info
└── pack

22 directories, 21 files
```
Not knowing what to do, I've just tried using `file` command on one of the files.
```bash
# file 01/533f718556a0e59f1467dae4fa462eed82c2a1
01/533f718556a0e59f1467dae4fa462eed82c2a1: zlib compressed data
```
It seems like all of them were zlib compressed data.
I could try to decompress all the data and see anything that looks like a flag.
I wrote a Python script that goes through each file and prints out the decompressed data.

```python
import os
import os.path
import zlib

for path, directory, files in os.walk('.'):
    for file in files:
        with open(os.path.join(path, file), 'rb') as f:
            content = f.read()
        print(zlib.decompress(content))
```

Then, one of the lines printed was:
```bash
b'blob 188\x00Rex: Meet at the old arcade basement for the secret hideout.\nJay: Ask Rusty at the door and use password picoCTF{REDACTED_HERE}.\nRex: Bring the decoder map so we can plan the route.\n'
```
which was the flag.
