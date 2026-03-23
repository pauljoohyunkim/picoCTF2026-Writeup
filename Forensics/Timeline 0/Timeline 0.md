# Timeline 0
Number of Points: 100

## Description
Can you find the flag in this disk image? Wrap what you find in the picoCTF flag format. Download the disk image here.

## Hints
* Create a Sleuthkit MAC timeline!
* Sloppy timestomping can yield strange (very old) timestamps

## Analysis & Solution
Start by downloading and decompressing the gzip.
```bash
wget https://challenge-files.picoctf.net/c_plain_mesa/aa1f8ba93409887e081435732d7037c45b30a8442853bf07c9e84fe4d0e0bc19/partition4.img.gz
gzip -d partition4.img.gz
```
As the hint suggests, create a Sleuthkit MAC timeline.
```bash
fls -r -m / partition4.img > body.txt
mactime -b body.txt > timeline.txt
```
Now, sloppy timestomping might mean the timestamp is either reset to Unix epoch (1970-01-01) or the latest date.
Let us check the first ever entry.
```bash
# head timeline.txt
Wed Jan 02 1985 02:00:00       41 macb r/rrw-r--r-- 0        0        4945     /bin/bcab
Tue Oct 19 2021 02:54:17      451 ma.. r/rrw-r--r-- 0        0        64994    /usr/share/apk/keys/alpine-devel@lists.alpinelinux.org-4a6a0840.rsa.pub
                              451 ma.. r/rrw-r--r-- 0        0        64995    /usr/share/apk/keys/alpine-devel@lists.alpinelinux.org-5243ef4b.rsa.pub
                              451 ma.. r/rrw-r--r-- 0        0        64996    /usr/share/apk/keys/alpine-devel@lists.alpinelinux.org-524d27bb.rsa.pub
                              451 ma.. r/rrw-r--r-- 0        0        64997    /usr/share/apk/keys/alpine-devel@lists.alpinelinux.org-5261cecb.rsa.pub
                              451 ma.. r/rrw-r--r-- 0        0        64998    /usr/share/apk/keys/alpine-devel@lists.alpinelinux.org-58199dcc.rsa.pub
                              451 ma.. r/rrw-r--r-- 0        0        64999    /usr/share/apk/keys/alpine-devel@lists.alpinelinux.org-58cbb476.rsa.pub
                              451 ma.. r/rrw-r--r-- 0        0        65000    /usr/share/apk/keys/alpine-devel@lists.alpinelinux.org-58e4f17d.rsa.pub
                              451 ma.. r/rrw-r--r-- 0        0        65001    /usr/share/apk/keys/alpine-devel@lists.alpinelinux.org-5e69ca50.rsa.pub
                              451 ma.. r/rrw-r--r-- 0        0        65002    /usr/share/apk/keys/alpine-devel@lists.alpinelinux.org-60ac2099.rsa.pub
```
We identified a weird file called `/bin/bcab`. To get this file, we extract the entire image file and navigate to it.

```bash
# 7z x partition4.img
# cd bin
# file bcab
bcab: ASCII text
# cat bcab
NzFtMzExbjNfMHU3MTEzcl9oM3JfNDNhMmU3YWYK
```
This looks like a base64 encoded string.
```bash
# base64 -d bcab
REDACTED_LEET_SPEAK_FLAG
```
We wrap this in "picoCTF{...}" and get our flag.
