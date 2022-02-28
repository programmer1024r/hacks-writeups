# Overthewire bandit

## General
```bash
# password directory
/etc/bandit_pass/
# file premision dir
/tmp/name123
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


### Passwords
- 17: `xLYVMN9WE5zQ5vHacb0sZEVqbrp7nBTn`
- 18: `kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd`
- 19: `IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x`
- 20: `GbKksEFF4yrVs6il55v6gwY5aVje5f0j` <br>
     in this level the idea is to use the bandit20-do program to cat the 20th password inside `/etc/bandit_pass/`.
- 21: `gE269g2h3mw3pwgrj0Ha9Uoqen1c9DGr` : 18:28.16
- 22: `Yk7owGAcWjwMVRwrTesJEwB7WVOiILLI` : 07:38.49

