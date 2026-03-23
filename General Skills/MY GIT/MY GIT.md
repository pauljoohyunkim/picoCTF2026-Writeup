# MY GIT
Number of Points: 50

## Description
I have built my own Git server with my own rules!

Additional details will be available after launching your challenge instance.

## Hints
* How do you specify your Git username and email?

## Analysis & Solution
After cloning and navigating to the challenge directory,
I found `README.md`
```
# MyGit

### If you want the flag, make sure to push the flag!

Only flag.txt pushed by ```root:root@picoctf``` will be updated with the flag.

GOOD LUCK!
```

While in Linux, privilege escalation is somewhat complicated,
in git, you could just say that you are whoever you are.

```bash
git config set user.name root
git config set user.email "root@picoctf"
```
then following the instruction in the README.md,
```bash
touch flag.txt
git add flag.txt
git commit -m "SOME COMMIT"
git push
```
After pushing, I got the flag

```bash
** WARNING: connection is not using a post-quantum key exchange algorithm.
** This session may be vulnerable to "store now, decrypt later" attacks.
** The server may need to be upgraded. See https://openssh.com/pq.html
git@foggy-cliff.picoctf.net's password: 
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 4 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 265 bytes | 265.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Author matched and flag.txt found in commit...
remote: Congratulations! You have successfully impersonated the root user
remote: Here's your flag: picoCTF{REDACTED_FLAG_HERE}
To ssh://foggy-cliff.picoctf.net:63434/git/challenge.git
   aa44ed9..4660833  master -> master
```
