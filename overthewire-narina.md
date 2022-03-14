# Narnia
### basic game info
```
levels: narnia0 narnia1...
narnia.labs.overthewire.org on port 2226

passwords at:
/etc/narnia_pass/
```
### level 1
#### attempts
1. looked for suspicious ports
2. cat the narnia1 password.
3. `You'll get the source code of each level to make it easier for you to spot the vuln and abuse it.`
   meaning there is a source code inside this machine lets find it... <br>
when running `find . | grep narnia`, you I found narnia0.c and its executable! <br>

the program is a simple buffer overflow the given buffer is 20 bytes and 
after that there is the target value. if you sucessfully overflow you get a shell.<br>
`setreuid(geteuid() geteuid())`, sets a real and effective user IDs of the calling process.<br>
The calling process will be the next level, because you can a SUID flag on the executable.

#### buffer overflow attempts
given a buff[20] and a value before it. the stack would look like this.<br>
| var      | high memory addr<br>
| buff[20] | low memory addr<br>

The program input 24 chars of string into buff, which is already a buffer overflow.

1. tried to enter `20*a+0xdeadbeef` <br> 
    val = edx0<br>
    buff = aaaaaaaaaaaaaaaaaaaa0xde<br>
    those there are the reverse of each other<br>
    target val = 0xdeadbeef = 3735928559<br>

    the input deadbeef should be reversed and without `0x`<br>
    this method worked partly, var = hex(beef) we need to eneter beef as hex reversed<br>
    `0xf0xe0xe0xb0xd0xa0xe0xd`
    
## solution
#### steps
- from the first input `20*a+0xdeadbeef`, we can infer two things little indian.




### Passwords


## Buffer overflow


## Terms
- real uid: the user ID
- effective uid: id can give the current user root privilage, with files.
- saved uid: id to lower your privilage inside a process.

