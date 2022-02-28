# Overthewire bandit

## General
```bash
# password directory
/etc/bandit_pass/
# file premision dir
/tmp/name123
# the cronjob scripts
/usr/bin/cronjob_bandit24.sh
```
#### Level 21 -> 22:
if you look inside the `/etc/cron.d` a list of files that specifiy the code execution for each user. 
```bash
cat /usr/bin/cronjob_bandit22.sh
# file output
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```
this file saves the password of level 22 into a tmp file, if we cat this file we get the 22th password.

### Level 23 -> 24
This script execute all files inside /var/spool/bandit24, and then removing them.
```Bash
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname
echo "Executing and deleting all scripts in /var/spool/$myname:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done
```
This bash script will create a file named `pass_for_bandit24` inside the `/tmp/name123`, and give every bandit user the option to read its data.<br>
```Bash
cd /var/spool/bandit24/
vim get_pass24.sh
```
inside the file
```Bash
#!/bin/bash
touch /tmp/pass24
chmod 644 /tmp/pass24
cat /etc/bandit_pass/bandit24 > /tmp/pass24
```
then try and enter the file with vim. the moment the file does not exsits, meaning bandit24 execute, you can cat the file `/tmp/pass24`

### Level 24 -> 25
In this level you need to execute oneliner brute force. 
I thought it was impossible for some reason and didn't tried it even when I thought about it /fp.
After looking online and understanding that it is possible, I created this code:
```Bash
for i in {0000..9999}; do echo "UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ $i"; done | nc localhost 30002
```

### Level 25 -> 26
```Bash
/bin/sh
/bin/dash
/bin/bash
/bin/rbash
/usr/bin/screen
/usr/bin/tmux
/usr/bin/showtext
```

`/usr/bin/showtext`
```Bash
#!/bin/sh

export TERM=linux

more ~/text.txt
exit 0
```
the shell showtext is not a shell, and it exit() after executing.
we need to change the default shell for bandit26, back to bash.
#### Attempts
1. we cannot change the `/etc/passwd` 
```
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
```
2. change the `/usr/bin/showtext` to execute `chsh -s /bin/bash`. I cannot change the shell file.
3. `usermod --shell /bin/bash bandit26`, the command is not recognized...

The solution is nuts! more will not exit until it will show all text. Meaning if we resize the terminal enough more will still be open. 
Then from inside more we can get into vim by pressing v. and then execute the commands:
```
set: shell=/bin/sh
:shell
```
from here we have a proper shell.

### Level 27 -> 28
we need to clone a repo from a localhost user. bandit27-git. if we login to this user we get a hint:
```
hint: ~/git-shell-commands should exist and have read and execute access.
```
a quick search reveals that it is a default hit from the git-shell 

#### Attempts
1. if we try to clone the repo, we don't have premission to create a new file called repo...
2. if we try to clone it inside `/tmp/name123` we don't have any premisions.
3. runing the `find . -perm /4000` to find all files that have a SUID flag we find 
```bash
./home/bandit19/bandit20-do
./home/bandit20/suconnect
./home/bandit32/uppershell
./home/bandit26/bandit27-do
./run/lock/find
./run/lock/hola
```
if you check th folder `/run/lock`, you can find very interseting files... 
First you can find a folder repo and the password for level 27 is inthere.
Second you can find a folder for level31 level30 level28 and bandit29, bandit30, bandit31.
In short we have now the passwords of level 28, 27, 29.
#### The intended solution
```bash
mktemp -d 
```
then clone in the tmp dir

#### Level 30 -> 31 
(always check everything that you don't know!)
the given repo has a file with one file. 
#### Attempts
1. went through the log, nothing.
2. went through the .git files, found a packed-ref, called secret.<br>
The packed-refs file:
```
3aefa229469b7ba1cc08203e5d8fa299354c496b refs/remotes/origin/master
f17132340e8ee6c159e0a4a6bc6f80e1da3b1aea refs/tags/secret
```
tried to checkout secret... no luck. packed-refs is a way for git to save a static tip of a branch for more efficiency.
#### Solusion
git tags are a way to save important notes 
```
git tag
git show <tag name>
```

### Level 31 -> 32
In this level we login with the uppercase shell. it converts every command to uppercase.

commands that we need to use in this level:<br>
man, sh
#### Attempts
1. input uppercase commands
2. try to change shell with the three methods that I tried in level 25.
3. try to look inside the uppershell file without success.
4. dash suppose to start faster then bash, then i tried to run it but no luck...

#### Solution
the idea is that the expression `$0` is the current running shell, meaning we can run it and go to the shell.


### Passwords
- 17: `xLYVMN9WE5zQ5vHacb0sZEVqbrp7nBTn`
- 18: `kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd`
- 19: `IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x`
- 20: `GbKksEFF4yrVs6il55v6gwY5aVje5f0j` <br>
     in this level the idea is to use the bandit20-do program to cat the 20th password inside `/etc/bandit_pass/`.
- 21: `gE269g2h3mw3pwgrj0Ha9Uoqen1c9DGr` : 18:28.16
- 22: `Yk7owGAcWjwMVRwrTesJEwB7WVOiILLI` : 07:38.49
- 23: `jc1udXuA1tiHqjIsL8yaapX5XIAI6i0n` : 05:54.12
- 24: `UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ` : 42:20.48+
- 25: `uNG9O58gUE7snukf3bvZ0rxhtnjzSGzG` : 21:35.33 + help
- 26: `5czgV9L3Xx8JPOyRbXh6lQbmIOWvPT6Z` : 1h + help
- 27: `3ba3118a22e93127a4ed485be72ef5ea` : 1m
- 28: `0ef186ac70e04ea33b4c1853d2526fa2` : help
- 29: `bbc96594b4e001778eee9975372716b2` : 20m
- 30: `5b90576bedb2cc04c86a9e924ce42faf` : 10m
- 31: `47e603bb428404d265f59c42920d81e5` : help
- 32: `56a9bf19c63d650ce78e6ec0354ee45e` : 04:04.45
- 33: `c9c3199ddf4121b10cf581a98d51caee` : help
