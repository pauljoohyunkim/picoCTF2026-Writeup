# Timeline 1
Number of Points: 300

## Description
Can you find the flag in this disk image? Wrap what you find in the picoCTF flag format. Download the disk image here.

## Hints
* Create a Sleuthkit MAC timeline!
* Look at recent timestamps
* Pay close attention to timestamps near an anti-forensic action
* Filter only new files by grepping for macb

## Analysis & Solution
Just as in [Timeline 1](../Timeline%200/Timeline%200.md), we download and create a MAC timeline.

In fact there is no difference in my opinion; you seek something abnormal.

Since the hint thankfully told us to filter by "macb", we shall do that.

```bash
# cat timeline.txt | grep macb
...
       111 macb r/rrw-r--r-- 0        0        33016    /etc/apk/world
     28160 macb r/rrw-r--r-- 0        0        4934     /lib/apk/db/scripts.tar
    411949 macb r/rrw-r--r-- 0        0        4945     /lib/apk/db/installed
        11 macb l/lrwxrwxrwx 0        0        64809    /usr/bin/crontab -> /bin/bbsuid
        11 macb l/lrwxrwxrwx 0        0        64872    /usr/bin/passwd -> /bin/bbsuid
        11 macb l/lrwxrwxrwx 0        0        64916    /usr/bin/traceroute -> /bin/bbsuid
        11 macb l/lrwxrwxrwx 0        0        64917    /usr/bin/traceroute6 -> /bin/bbsuid
        11 macb l/lrwxrwxrwx 0        0        64936    /usr/bin/vlock -> /bin/bbsuid
         3 macb l/lrwxrwxrwx 0        0        64945    /usr/bin/ex -> vim
         3 macb l/lrwxrwxrwx 0        0        67318    /usr/bin/rview -> vim
         3 macb l/lrwxrwxrwx 0        0        67319    /usr/bin/rvim -> vim
         3 macb l/lrwxrwxrwx 0        0        67320    /usr/bin/view -> vim
        49 macb r/rrw-r--r-- 0        0        32716    /etc/chat
      1024 macb d/drwxr-xr-x 0        0        33017    /lib/rc/cache
         8 macb r/rrw-r--r-- 0        0        33020    /lib/rc/cache/softlevel
         9 macb r/rrw------- 0        0        4943     /root/.ash_history
        32 macb r/rr-------- 0        0        65278    /var/lib/seedrng/seed.credit
```
This time, I can see `/etc/chat`.
Extracting the image and inspecting that file, we get
```bash
# cat etc/chat 
NTczNDE3aDEzcl83aDRuXzdoM18xNDU3XzU4NTI3YmIyMjIK
```
Another base64 encoded text!

```bash
# bash64 -d etc/chat
REDACED_FLAG_HERE
```
