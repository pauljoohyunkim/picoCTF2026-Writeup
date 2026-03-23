# DISKO 4
Number of Points: 200

## Description
Can you find the flag in this disk image? This time I deleted the file! Let see you get it now! Download the disk image here.

## Hints
* How would you look for deleted files?

## Analysis & Solution
Let us start by downloading the compressed image file and decompressing it.
```bash
wget https://challenge-files.picoctf.net/c_plain_mesa/c7ab8389098be28cbd419686b9ec4a2b969a06ecd0e11cb9ae6af9acf4e063b1/disko-4.dd.gz
gzip -d disko-4.dd.gz
```

We are dealing with deleted files, so blindly extracting using 7z like I did in [Forensics Git 0](../Forensics%20Git%200/Forensics%20Git%200.md) would not work.

After Googling on how to recover deleted files from a disk image, I found a utility called `testdisk`.

Using `testdisk`, we can see the filesystem with the files, including the ones that are "marked for deletion".
```bash
# testdisk disko-4.dd
# Press enter a few times until you get
TestDisk 7.2, Data Recovery Utility, February 2024
Christophe GRENIER <grenier@cgsecurity.org>
https://www.cgsecurity.org

Disk disko-4.dd - 104 MB / 100 MiB - CHS 800 8 32
     Partition                  Start        End    Size in sectors
   P FAT32                    0   0  1   799   7 32     204800 [NO NAME]

Boot sector
OK

Backup boot sector
OK

Sectors are identical.

A valid FAT Boot sector must be present in order to access
any data; even if the partition is not bootable.

# Then do '[ List ]', then navigate
TestDisk 7.2, Data Recovery Utility, February 2024
Christophe GRENIER <grenier@cgsecurity.org>
https://www.cgsecurity.org
   P FAT32                    0   0  1   799   7 32     204800 [NO NAME]
Directory /log

>drwxr-xr-x     0     0         0 31-Mar-2025 15:31 .
 drwxr-xr-x     0     0         0 31-Mar-2025 15:31 ..
 drwxr-xr-x     0     0         0 31-Mar-2025 15:31 private
 drwxr-xr-x     0     0         0 31-Mar-2025 15:31 sysstat
 drwxr-xr-x     0     0         0 31-Mar-2025 15:31 stunnel4
 drwxr-xr-x     0     0         0 31-Mar-2025 15:31 mysql
 drwxr-xr-x     0     0         0 31-Mar-2025 15:31 inetsim
 drwxr-xr-x     0     0         0 31-Mar-2025 15:31 installer
 -rwxr-xr-x     0     0      3135 31-Mar-2025 15:31 vmware-vmsvc-root.2.log
 -rwxr-xr-x     0     0     18671 31-Mar-2025 15:31 kern.log.4.gz
 -rwxr-xr-x     0     0     21322 31-Mar-2025 15:31 Xorg.0.log
 -rwxr-xr-x     0     0       195 31-Mar-2025 15:31 vmware-network.4.log
 -rwxr-xr-x     0     0         0 31-Mar-2025 15:31 boot.log
 -rwxr-xr-x     0     0     46637 31-Mar-2025 15:31 syslog.3.gz
 -rwxr-xr-x     0     0     19624 31-Mar-2025 15:31 vmware-vmtoolsd-root.log
 -rwxr-xr-x     0     0    299509 31-Mar-2025 15:31 daemon.log
 -rwxr-xr-x     0     0    704371 31-Mar-2025 15:31 messages
 -rwxr-xr-x     0     0       194 31-Mar-2025 15:31 alternatives.log.2.gz
 -rwxr-xr-x     0     0     24374 31-Mar-2025 15:31 debug
 drwxr-xr-x     0     0         0 31-Mar-2025 15:31 lightdm
 -rwxr-xr-x     0     0       193 31-Mar-2025 15:31 vmware-network.6.log
 -rwxr-xr-x     0     0         0 31-Mar-2025 15:31 alternatives.log
                                                   Next
Use Left arrow to go back, Right to change directory, 'h' to hide deleted files
    'q' to quit, ':' to select the current file, 'a' to select all files
    'C' to copy the selected files, 'c' to copy the current file
```

It might not show up on this markdown document, but if you scroll down, `dont-delete.gz` file is marked in red.
Press 'c' then 'c' to copy to my local directory. Then press 'q' until you exit out of the program.

You should now see a directory called `log` with a file called `dont-delete.gz`.

Uncompressing and reading the file, you get the flag.
