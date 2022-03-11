# Hardware

#### oneline beautifier javascript
https://beautifier.io/ <br>

The CTF 
https://app.hackthebox.com/challenges/207

#### meta.json
a configuration file written in json
#### JSON
a file format, for storing data. 

### Terms
- interval -> a set of real numbers between the startpoint - endpoint of the interval.

## Leads
- rowsSettings have is hidden settings some are false...
- 
The big picture:<br> 
meta.json - the config file for the digital-0.bin
digital-0.bin - the debugging program that captured something...

## Writeup
- when you unzip the file you get .sal file..
- unzip the file agian and you get a meta.json and digital-0.bin file.
- file digital-0.bin doesn't show us stuff so hexdump the data<br>
    `hexdump -C digital-0.bin | head -1` returns the file format (SALEAE), 
- Saleae is a hardwer that can analyze voltage and convert it to binary file.
- I installed the Logic analyzer and run the app image
```
dlopen(): error loading libfuse.so.2

AppImages require FUSE to run.
You might still be able to extract the contents of this AppImage
if you run it with the --appimage-extract option.
See https://github.com/AppImage/AppImageKit/wiki/FUSE
for more information
```
you need to add fuse lib program.
##### Fuse
AppImages require FUSE to run. 
Filesystem in Userspace (FUSE) is a system that lets non-root users mount filesystems.
##### --------------------------------------------------------------------------------------------------------------------------------------------------
I was not able to run the program... a socket error...<br>
https://lesbianunix.dev/posts/hackthebox-debugginginterface/
